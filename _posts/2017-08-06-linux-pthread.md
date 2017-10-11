---
layout: post
title: "Linux Pthread API 測試筆記"
description: "Linux Pthread API test"
categories: [Linux]
tags: [Linux, C]
redirect_from:
  - /2017/08/06/
---

近日工作上需要讓程式同時能處理多個工作, 就想起了以前學過的pthread API, 原本使用fork來工作, 但這會比執行緒花掉更多的系統資源, 因此決定來複習一下

## 建立執行緒

當呼叫pthread_create時會建立一個新的執行緒出來

```
/* pthread_create 函數原型 */
int pthread_create (pthread_t *__newthread, 
                    const pthread_attr_t *__attr,
                    void *(*__start_routine) (void *),
                    void *__arg);
```

* __newthread: 執行緒物件
* __attr: 執行緒屬性 沒有特別定義屬性時 可以使用 (pthread_attr_t *)NULL
* __start_routine: 執行緒函數指標
* __arg: 函數指標參數 沒有需要傳入的參數 可以傳入 (void *)NULL

## 合併執行緒

當要合併執行緒可以呼叫pthread_join來完成 在合併前此執行緒會被暫停必須等到另一個執行緒執行完畢才會完成合併動作

```
int pthread_join (pthread_t __th, void **__thread_return);
```

* __th: 被合併目標
* __thread_return: 被合併目標的回傳值

## 結束執行緒

要關閉執行緒時可以呼叫pthread_exit來完成

```
/* pthread_exit 函數原型 */
void pthread_exit (void *__retval);
```

* __retval: 執行緒結束回傳的值

## 小小練習

```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <pthread.h>

void *task01(void *args)
{
  char a = 0;
  for(a=0; a<3; a++) {
    printf("task01 %d\n", a);
    sleep(2);
  }
  pthread_exit(NULL);
}

void *task02(void *args)
{
  char a = 0;
  for(a=0; a<5; a++) {
    printf("task02 %d\n", a);
    sleep(1);
  }
  pthread_exit(NULL);
}

int main(int argc, char *argv[])
{
  pthread_t pth01, pth02;

  pthread_create(&pth01, NULL, task01, NULL);
  pthread_create(&pth02, NULL, task02, NULL);

  pthread_join(&pth01, NULL);
  pthread_join(&pth02, NULL);
  
  return EXIT_SUCCESS;
}
```

這裡必須注意一下 因為我們使用了pthread動態函式庫 所以再編譯時期要加入pthread函數庫喔!!!

```
user@ubuntu:~$ gcc -o pthread.o pthread.c -l pthread
```

執行結果
```
task01 0
task02 0
task02 1
task01 1
task02 2
task02 3
task01 2
task02 4
```