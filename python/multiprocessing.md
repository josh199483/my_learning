[網站寫得很詳細](http://wiki.jikexueyuan.com/project/explore-python/Process-Thread-Coroutine/thread.html)
## 使用process池來管理process
```
def task(pid):
    # do something
    return result
def main():
    multiprocessing.freeze_support()   # Windows要加上這句，避免RuntimeError
    pool = multiprocessing.Pool()     
    cpus = multiprocessing.cpu_count()
    results = []
    for i in range(0, cpus):
        result = pool.apply_async(task, args=(i,))
        results.append(result)
    pool.close()
    pool.join()
    for result in results:
        print(result.get())
```
pool.apply_async 採用異步方式調用 task，pool.apply 則是同步方式調用。同步方式意味着下一個 task 需要等待上一個 task 完成後才能開始運行，這顯然不是我們想要的功能，所以採用異步方式連續的提交任務<br>
這裡有個疑問，為什麼不直接在 for loop中直接 result.get()呢？<br>
這是因為pool.apply_async之後的語句都是阻塞執行的，調用 result.get() 會等待上一個任務執行完之後才會分配下一個任務。<br>
事實上，獲得return值的過程最好在process pool回收之後進行，避免阻塞後面的語句<br>
