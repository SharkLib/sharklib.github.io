---
date: 2020-01-01 14:48:05
layout: post
title: Qt Thread Pool
subtitle: Using a pool to share thread
description: >-
  A very useful collection for learn QT multiple thread coding.
image: >-
  /assets/img/Qtcreator-overview.png
optimized_image: >-
  /assets/img/Qtcreator-overview.png
category: Qt
tags:
  - welcome
  - blog
  - Qt
author: Ron
paginate: true
---
 ## Qt Thread Pool 

   In traditionly, using a thread has three phases: create(P1), excute(P2), distroy(P3).
It is obviously a thread occupies time is P1+ P3, and the thread excute a task time is P2.
It means if the task is a short-term task, the thread itself is very expensive.
And if the count of threads is more than the CPU count, CPU need time to awake a thread and cost time too.
So we need a thread pool, in the pool we will create enough threads according to the CPU number.

Here I will present how to use a QThread and std::thread to implementate a thread pool.

  ### head file
    
    
#### Class MyThread 
```
  #include < QThread>
#include < QObject>

class MyThread: public QThread
{
public:
  MyThread(QObject *parent = Q_NULLPTR);

  void (*work)(QString);

  QString _thread_name;

  // QThread interface
protected:
  virtual void run();
};
 ```
    
```
  // my first program in C++
  #include < QThread>
#include < QObject>
class MyPool: public QObject
{
  Q_OBJECT
public:
  MyPool(int n =10,QObject *o = nullptr);
  virtual ~MyPool();
  void addThread(MyThread* td);

  void setWork( void (*fun)(QString) );
protected:
  void slotStarted();
  void slotFinished();
private:

  QList< MyThread*> _lt;
  QList< void (*)(QString)> _ltTasks;
};
  </pre>


    </div>


 <h2>cpp file</h2>
    <div class="box" style="background-color:#bbb">
    
   
    <pre class='brush: cpp'>
  #include < QDebug>

MyThread::MyThread(QObject *parent )
  :QThread(parent)
{
  work = nullptr;
}


void MyThread::run()
{
  if(work)
    work(_thread_name);
}

MyPool::MyPool(int n,QObject *o)
  :QObject(o)
{
  for(int i=0; i< n; i++)
  {
    auto th = new MyThread();
   _lt  <<  th;
    connect(th, &QThread::started, this, &MyPool::slotStarted);
    connect(th, &QThread::finished, this, &MyPool::slotFinished);
  }
}

MyPool::~MyPool()
{

}

void MyPool::addThread(MyThread *td)
{
  _lt << td;
}

void MyPool::setWork(void (*fun)(QString))
{

  for (auto th: _lt)
  {
    if(th->isRunning() == false)
    {
      th->work = fun;
      th->_thread_name = "Test" + QString::number(_lt.indexOf(th));
      qDebug() << __FUNCTION__ << "add task to Thread " << _lt.indexOf(th);
      th->start();
      return;
    }
  }
  qDebug() << __FUNCTION__ << "All Thread are busy, put to waiting list";
  _ltTasks<< fun;
}

void MyPool::slotStarted()
{
  auto th = dynamic_cast< MyThread*>( sender());
  qDebug() << __FUNCTION__ << " Thread are started"<< _lt.indexOf(th);
}

void MyPool::slotFinished()
{
  auto th = dynamic_cast< MyThread*>( sender());
  qDebug() << __FUNCTION__ << " Thread are finished" << _lt.indexOf(th);
  // start waitting task
  if( _ltTasks.count() > 0)
  {
    auto task = _ltTasks[0];
    _ltTasks.pop_front();
    th->work = task;
    qDebug() << __FUNCTION__ << " Thread start waiting task " << _lt.indexOf(th);
    th->start();
  }
  int n = 0;
  for(auto th: _lt)
  {
    if(th->isRunning())
      n++;
  }
  qDebug() << __FUNCTION__ << " Runing thread " << n << "Total Thread" << _lt.count();
}
```

#### main.cpp
  ```


void work1(QString strdd)
{
  qDebug() << strdd << __FUNCTION__ << "start";
  for(int n=0; n< 100; n++)
  {
    QThread::sleep(1);
    if(n%10==0)
      qDebug() << str << __FUNCTION__ << n;
  }
  qDebug() << str << __FUNCTION__ << "end";
}

void work2(QString str)
{
  qDebug() << str << __FUNCTION__ << "start";
  for(int n=0; n< 100; n++)
  {
    QThread::sleep(2);
    if(n%10==0)
      qDebug() << str << __FUNCTION__ << n;
  }
  qDebug() << str << __FUNCTION__ << "end";
}

void work3(QString str)
{
  qDebug() << str << __FUNCTION__ << "start";
  for(int n=0; n< 100; n++)
  {
    QThread::sleep(3);
    if(n%10==0)
      qDebug() << str << __FUNCTION__ << n;
  }
  qDebug() << str << __FUNCTION__ << "end";
}

void work4(QString str)
{
  qDebug() << str << __FUNCTION__ << "start";
  for(int n=0; n< 100; n++)
  {
    QThread::sleep(4);
    if(n%10==0)
      qDebug() << str << __FUNCTION__ << n;
  }
  qDebug() << str << __FUNCTION__ << "end";
}

void work5(QString str)
{
  qDebug() << str << __FUNCTION__ << "start";
  for(int n=0; n< 100; n++)
  {
    QThread::sleep(5);
    if(n%10==0)
      qDebug() << str << __FUNCTION__ << n;
  }
  qDebug() << str << __FUNCTION__ << "end";
}

    int main()
    {
      MyPool _pool;
      _pool.setWork(&work1);
  _pool.setWork(&work1);
  _pool.setWork(&work2);
  _pool.setWork(&work3);
  _pool.setWork(&work2);
  _pool.setWork(&work3);
  _pool.setWork(&work4);
  _pool.setWork(&work5);
  _pool.setWork(&work4);
  _pool.setWork(&work5);
  _pool.setWork(&work1);
  _pool.setWork(&work1);
  _pool.setWork(&work2);
  _pool.setWork(&work3);
  _pool.setWork(&work2);
  _pool.setWork(&work3);
  _pool.setWork(&work4);
  _pool.setWork(&work5);
  _pool.setWork(&work4);
  _pool.setWork(&work5);

  return 0;
    }
```

#### Output:
  ```
  
   setWork add task to Thread  0
setWork add task to Thread  1
setWork add task to Thread  2
setWork add task to Thread  3
setWork add task to Thread  4
setWork add task to Thread  5
setWork add task to Thread  6
setWork add task to Thread  7
"Test2" work2 start
"Test5" work3 start
"Test1" work1 start
setWork add task to Thread  8
"Test6" work4 start
"Test4" work2 start
"Test7" work5 start
setWork add task to Thread  9
"Test0" work1 start
"Test8" work4 start
"Test9" work5 start
"Test3" work3 start
slotStarted  Thread are started 2
slotStarted  Thread are started 4
slotStarted  Thread are started 1
slotStarted  Thread are started 5
slotStarted  Thread are started 6
slotStarted  Thread are started 7
slotStarted  Thread are started 0
slotStarted  Thread are started 8
slotStarted  Thread are started 9
slotStarted  Thread are started 3
"Test0" work1 0
...
"Test4" work2 60
"Test1" work1 end
"Test0" work1 end
slotFinished  Thread are finished 1
slotFinished  Runing thread  8 Total Thread 10
slotFinished  Thread are finished 0
slotFinished  Runing thread  8 Total Thread 10
"Test2" work2 70
...
"Test7" work5 40
"Test4" work2 end
"Test2" work2 end
slotFinished  Thread are finished 4
slotFinished  Runing thread  6 Total Thread 10
slotFinished  Thread are finished 2
slotFinished  Runing thread  6 Total Thread 10
"Test5" work3 70
...
"Test6" work4 70
"Test3" work3 end
"Test5" work3 end
slotFinished  Thread are finished 3
slotFinished  Runing thread  4 Total Thread 10
slotFinished  Thread are finished 5
slotFinished  Runing thread  4 Total Thread 10
"Test9" work5 60
...
"Test8" work4 90
"Test8" work4 end
"Test6" work4 end
slotFinished  Thread are finished 8
slotFinished  Runing thread  2 Total Thread 10
slotFinished  Thread are finished 6
slotFinished  Runing thread  2 Total Thread 10
"Test7" work5 80
"Test9" work5 80
"Test7" work5 90
"Test9" work5 90
"Test7" work5 end
"Test9" work5 end
slotFinished  Thread are finished 7
slotFinished  Runing thread  0 Total Thread 10
slotFinished  Thread are finished 9
slotFinished  Runing thread  0 Total Thread 10
```


