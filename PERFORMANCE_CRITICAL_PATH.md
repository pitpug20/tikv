https://www.tiktok.com/@rosie.u.n/live?enter_from_merge=general_search&enter_method=others_photo&search_id=20241011233553CDDA4917D259650BBAFF&search_keyword=rosie%20k%E1%BB%83%20chuy%E1%BB%87n&search_result_id=72118334972&search_type=general
Tiktok.develope
https://developers.tiktok.com/


# Performance Critical Path of user requests

There're many files whose functions are in the critical path of read or write requests. They're so important to the overall performance that any regression will directly impact user experience. We add a comment `#[PerformanceCriticalPath]` to highlight these critical files. Please note that this is the best-effort work and some files in critical path may not be marked. But if a file is marked,  please pay special attention when you change its code.

Here're some typical mistakes that should be avoided in the `#[PerformanceCriticalPath]` files:

* Unnecessary synchronous I/O. Here 'unnecessary' means it's not a MUST for serving the current user request. For example, on_gc_snap() in peers.rs should spin off its I/O related work to background thread.
* Verbose logging with info or above log level.
* Global lock.
* Long tasks that do not have to be synchronous (Could be done in background thread instead).

Below is the performance map that would help you to understand how read/write requests are executed inside TiKV:

![performance map](images/tikv_map.png)

/
Start