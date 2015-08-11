___
layout: post
title: "Where to Put Rounding?"
categories: python, multithread
___

Thanks to the flaws of the universe, our multi-threaded application doesn't use 100% of the cores it has access to. Sometimes a thread lags and has to be killed, and restarted with a new job. The act of killing and restarting it takes time, and ultimately I've observed that at any given moment in time, 90% of the cores are in use. In an effort to eke out that extra 10% performance, I realized I could have the thread spawner add a buffer to the number of threads it creates, of about 10% (
`ceil(0.1*thread_count)` for example), and with the threads missing there should be an average of 100% utilization. The extra threads won't be hurting the speed when they're present, of course, because that's how MT works. The problem is, where do I put the round up?

I could put it in the function that generates threads, but that creates a strange contract. It accepts an integer, called thread_count, which is the number of threads to create. Increasing that by 10% is secret and magic. I could do it in the function that calls the thread creator, but it ALSO accepts thread_count, so the same contract is broken. This happens a few times, bubbling up and up, until you reach the initial function call of the application. Life is super messy. I decided
to put the rounding in the body of the initial function call. That way, usage of "library" functions work the way one would expect, but the actual application handles the messiness of the universe. Sure, it breaks some obviousness in the contract, but really, to that function, thread_count is the number of threads you want active, not the number of threads to make, right? Right.
