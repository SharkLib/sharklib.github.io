---
date: 2020-01-01 14:48:05
layout: post
title: STL Thread Pool
subtitle: Using a pool to share thread
description: >-
  A very useful collection for learn  multiple thread coding.
image: >-
  /assets/img/Qtcreator-overview.png
optimized_image: >-
  /assets/img/Qtcreator-overview.png
category: blog
tags:
  - welcome
  - blog
  - Qt
author: Ron
paginate: true
---
 ### std Thread Pool 

   In traditionly, using a thread has three phases: create(P1), excute(P2), distroy(P3).
It is obviously a thread occupies time is P1+ P3, and the thread excute a task time is P2.
It means if the task is a short-term task, the thread itself is very expensive.
And if the count of threads is more than the CPU count, CPU need time to awake a thread and cost time too.
So we need a thread pool, in the pool we will create enough threads according to the CPU number.

Here I will present how to use a std::thread and std::thread to implementate a thread pool.

```
  #include < thread>
#include < iostream>
#include < list>
#include < vector>
#include < string>
#include < mutex>
#include < future>

using namespace std;
mutex gMutex;
void abc()
{
    cout << "test" << endl;
}
class Pool
{
public: 
    Pool (){ cout << "Pool constructed" << endl;}
    virtual ~Pool(){cout << "Pool distroied" << endl;}

    static void run(Pool* pl)
    {
        while(pl->stop == false)
        {
           lock_guard< mutex > mLock( gMutex ); 
            cout << "Lock " << std::this_thread::get_id();
            if(pl->ltTasks.size() !=0)
            {
                auto tt = pl->ltTasks.back();
                tt("abc");
                pl->ltTasks.pop_back();
            }
            else
            {
                //std::thread::sleep_for(100);
                cout << "no Task" << endl;
                // gMutex.unlock();
                return;;
            }
            cout << "Unlock " << std::this_thread::get_id();
           // gMutex.unlock();
            
            
        }
    }
    vector< thread > ltThread;
    vector< std::function < int( string ss ) > > ltTasks;
    bool stop;
};
int work(string str)
{
    cout << "test == "  << str << endl;
    cout <<  std::this_thread::get_id() << endl;
    return 0;
}
int main()
{
    Pool pl;

     for(unsigned i=0;i<10;++i)
            {
                pl.ltTasks.push_back(&work);
                cout << "add == "  << "str" << endl;
                std::thread th (&Pool::run,&pl );
                pl.ltThread.push_back( move(th) );  // 9
               
            }
    cout << "Test start" << endl;
    for(unsigned i=0;i<10;++i)
    {
         pl.ltThread[i].join();
    }
   
    pl.ltTasks.push_back(&work);
    cout << "end" << endl;
    vector< thread > lt;
    //thread abb( abc);
    //lt.push_back( move(abb));
    //abb.join();
    return 0;
}
```


