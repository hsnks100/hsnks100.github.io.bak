---
layout: post
title: "Design Pattern Observer"
author: 뻘짓마스터(hsnks100@gmail.com)
date: 2017-07-12 14:46 +0900
tags: observer design pattern
category: programming
comments: true
---
* table of contents
{:toc}



# Observer Pattern



``` java 
import java.util.List;
import java.util.ArrayList;

 class DataSheetView {
    private ScoreRecord scoreRecord;
    private int viewCount;

    public DataSheetView(ScoreRecord scoreRecord, int viewCount) {
        this.scoreRecord = scoreRecord;
        this.viewCount = viewCount;
    }

    public void update() { // 점수의 변경을 통보 받음
        List<Integer> record = scoreRecord.getScoreRecord(); // 점수를 조회함
        displayScores(record, viewCount); // 조회된 점수를 viewCount만큼 출력함
    }

    private void displayScores(List<Integer> record, int viewCount) {
        System.out.print("List of " + viewCount + " entries: ");
        for (int i = 0; i < viewCount && i < record.size(); i++) {
            System.out.print(record.get(i) + " ");
        }
        System.out.println();
    }
}

 class ScoreRecord {
    private List<Integer> scores = new ArrayList<Integer>(); // 점수를 저장함
    private DataSheetView dataSheetView; // 목록 형태로 점수를 출력하는 클래스

    public void setDataSheetView(DataSheetView dataSheetView) {
        this.dataSheetView = dataSheetView;
    }

    public void addScore(int score) { // 새로운 점수를 축함
        scores.add(score); // scores 목록에 주어진 점수를 추가함
        dataSheetView.update(); // scores가 변경됨을 통보함
    }

    public List<Integer> getScoreRecord() {
        return scores;
    }
}

public class Main {
    public static void main(String[] args){
        ScoreRecord sr = new ScoreRecord();

        sr.setDataSheetView(new DataSheetView(sr, 10));
        sr.addScore(2);
        sr.addScore(3);
        sr.addScore(4);
        sr.addScore(5);
        sr.addScore(6); 
    }
} 
```

위의 코드는 문제가 있다. 잘 보면 ScoreBoard 는 DataSheetView 에 notify 를 하고 있는데 만약 DataSheeView 가 아닌 다른 View 가 생긴다면 

ScoreRecord 의 구조를 고쳐야 하는 문제가 있다.

observer pattern 으로 고치면 다음과 같다.
``` java

import java.util.ArrayList;
import java.util.ArrayList;
import java.util.List;
import java.util.List;
import java.util.Collections;
import java.util.List;

class DataSheetView implements Observer{
    private ScoreRecord scoreRecord;
    private int viewCount;

    public DataSheetView(ScoreRecord scoreRecord, int viewCount) {
        this.scoreRecord = scoreRecord;
        this.viewCount = viewCount;
    }

    public void update() { // 점수의 변경을 통보 받음
        List<Integer> record = scoreRecord.getScoreRecord(); // 점수를 조회함
        displayScores(record, viewCount); // 조회된 점수를 viewCount만큼 출력함
    }

    private void displayScores(List<Integer> record, int viewCount) {
        System.out.print("List of " + viewCount + " entries: ");
        for (int i = 0; i < viewCount && i < record.size(); i++) {
            System.out.print(record.get(i) + " ");
        }
        System.out.println();
    }
}


class MinMaxView implements Observer{ // 전체 점수가 아니라 최소/최대값만을 출력하는 클래스
    /**
     * @uml.property  name="scoreRecord"
     * @uml.associationEnd  multiplicity="(1 1)"
     */
    private ScoreRecord scoreRecord;

    public MinMaxView(ScoreRecord scoreRecord) {
        this.scoreRecord = scoreRecord;
    }

    public void update() {
        List<Integer> record = scoreRecord.getScoreRecord();
        displayMinMax(record); // 최소/최대값만을 출력
    }

    private void displayMinMax(List<Integer> record) {
        int min = Collections.min(record, null);
        int max = Collections.max(record, null);
        System.out.println("Min: " + min + " Max: " + max);
    }
}

interface Observer{
    public void update(); 
}

interface DataPublisher{
    public void attach(Observer observer);
        public void detach(Observer observer);
        public void notifyObservers();
}
class ScoreRecord implements DataPublisher{
    private ArrayList<Observer> observers = new ArrayList<>();
        private List<Integer> scores = new ArrayList<Integer>(); // 점수를 저장함 
    
        public void addScore(int score) { // 새로운 점수를 축함
            scores.add(score); // scores 목록에 주어진 점수를 추가함 
            notifyObservers(); 
        }
    
        public List<Integer> getScoreRecord() {
            return scores;
        }
    
        
        
        @Override
        public void attach(Observer observer) {
            observers.add(observer); 
        }
    
        @Override
        public void detach(Observer observer) {
            int index = observers.indexOf(observer);
                observers.remove(index); 
        }
    
        @Override
        public void notifyObservers() {
            for(Observer ob : observers){
                ob.update();
            } 
        }
}
public class TestObserver {
    public static void main(String[] args){
        ScoreRecord sr = new ScoreRecord();

        sr.attach(new DataSheetView(sr, 10));
        sr.attach(new MinMaxView(sr));

        sr.addScore(2);
        sr.addScore(3);
        sr.addScore(4);
        sr.addScore(5);
        sr.addScore(6);

    }
} 

```





