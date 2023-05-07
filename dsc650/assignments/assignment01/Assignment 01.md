---
title: Assignment 1
subtitle: Computer performance, reliability, and scalability calculation
author: Josh Greenert
---

## 1.2 

#### a. Data Sizes

| Data Item                                  | Size per Item | 
|--------------------------------------------|--------------:|
| 128 character message.                     |     128 Bytes |
| 1024x768 PNG image                         |      2.359 MB | # 1024 pixels x 768 pixels x 3 bytes per pixel = 2,359,296 bytes / 1,048,576 
| 1024x768 RAW image                         |      1.311 MB | # 1024 pixels x 768 pixels x (14 bpp / 8) = 1,374,336 bytes / 1,048,576 
| HD (1080p) HEVC Video (15 minutes)         |   1072.884 MB | # 10mbps = 10,000kbps X 900 seconds / 8 bits = 1,125,000,000 bytes / 1,048,576
| HD (1080p) Uncompressed Video (15 minutes) | 14,089.966 MB | # 1920 x 1080 x 3 bytes x 30 frames x 90 sec = 14,774,400,000 bytes / 1,048,576
| 4K UHD HEVC Video (15 minutes)             |  2,682.209 MB | # 25mbps = 25000kbps x 900 sec / 8 bits p byte = 2,812,500,000 bytes / 1,048,576
| 4k UHD Uncompressed Video (15 minutes)     | 60,706.421 MB | # 3840 x 2160 x 3 bytes x 30 frames x 900 sec = 63,655,296,000 bytes / 1,048,576
| Human Genome (Uncompressed)                |    760.939 GB | # 3.2 billion base pairs x 2 bits / 8 bits p byte / 1,024 b / 1,024 kb / 1,024 kb

#### b. Scaling
<!--
Assume each hard drive is 10TB.  By default, HDFS stores three copies of each piece of data, so you will need to triple the amount storage required
Twitter statistics estimates 500 million tweets are sent each day. For simplicity, assume each tweet is 128 characters.  See the Snappy Github repository for estimates of Snappy's performance.
Instagram statistics estimates over 100 million videos and photos are uploaded to Instagram every day. Assume that 75% of those items are 1024x768 PNG photos.
YouTube statistics estimates 500 hours of video is uploaded to YouTube every minute. For simplicity, assume all videos are HD quality encoded using HEVC at 30 frames per second. 
-->

|                                           | Size                      | # HD | 
|-------------------------------------------|---------:                 |-----:|
| Daily Twitter Tweets (Uncompressed)       |      0.192 TB             |  1   | # 128 bytes x 500 million tweets = 64,000,000,000 bytes 
                                                                                 # 64,000,000,000 bytes  / 1,000,000,000,000 x 3 = 0.192 TB
| Daily Twitter Tweets (Snappy Compressed)  |      0.096 TB             |  1   | # Size of compressed tweet = 128 bytes / 2 = 64 bytes
                                                                                 # 500 million tweets x 64 bytes = 
                                                                                 # 32,000,000,000 bytes / 1,000,000,000,000 x 3
                                                                                 # 0.096 TB
| Daily Instagram Photos                    |    53.0775 TB             |  54  | # img = 2.359 MB x 75,000,000 = 176,925,000 mb / 1,000,000 x 3
                                                                                 # 530.775 TB / 10 TB = 53.0775 TB
| Daily YouTube Videos                      |   9,269.72 TB             |  927 | # 500 hours x 60 minutes x 24 hours = 720,000 hours of video
                                                                                 # 1072.884 MB x 4 = 4,291.536 MB per hour size
                                                                                 # 4,291.536 MB x 720,000 hours / 1,000,000 x 3 = 9,269.72 TB
| Yearly Twitter Tweets (Uncompressed)      |      70.08 TB             |   8  | # 0.192 TB x 365 (inclusive of 3x) = 70.08 TB
| Yearly Twitter Tweets (Snappy Compressed) |      35.04 TB             |   4  | # 0.096 TB x 365 (inclusive of 3x) = 35.04 TB
| Yearly Instagram Photos                   |  19,373.29 TB             |1,938 | # 53.0775 TB x 365 (inclusive of 3x) = 19,373.29 TB
| Yearly YouTube Videos                     |3,383,447.80 TB            |338,345| # 9,269.72 TB x 365 (inclusive of 3x) = 3,383,447.80 TB

#### c. Reliability
<!-- 
Using the yearly estimates from the previous part, estimate the number of hard drive failures per year using data from Backblaze's hard drive statistics.
https://www.backblaze.com/blog/backblaze-drive-stats-for-q3-2022/
https://www.backblaze.com/b2/hard-drive-test-data.html

-->
|                                    |  # HD | # Failures | # Failures 10TB|
|------------------------------------|- ----:|-----------:|---------------:|
| Twitter Tweets (Uncompressed)      |  8    |  0.1096    |  0.2424        | # 8 x 0.0137 = 0.1096 OR 8 x 0.0303 = 0.2424
| Twitter Tweets (Snappy Compressed) |  4    |  0.0548    |  0.1212        | # 4 x 0.0137 = 0.0548 OR 4 x 0.0303 = 0.1212
| Instagram Photos                   |  1,938|  26.55     |   58.72        | # 1,938 x 0.0137 = 26.5506 OR 1,938 x 0.0303 = 58.7214
| YouTube Videos                     |338,345| 4,635.33   |  10,251.85     | # 338,345 x 0.0137 = 4,635.33 OR 338,345 x 0.0303 = 10,251.85

#### d. Latency
<!-- 
Provide estimates of the one way latency for each of the following items. Please explain how you arrived at the estimates for each item by citing references or providing calculations 

Based on cacluating distances and the speed of light.
latency (ms) = (distance x 2 / speed of light) x 1000  <- This is based on two-way; we need one-way
speed of light = 299,792.458 ms
*easier convserions with metric
-->
|                           | One Way Latency      |
|---------------------------|---------------------:|
| Los Angeles to Amsterdam  | 29.80 ms             | # (8934 km / 299,792.458) x 1000 = 29.80 ms
| Low Earth Orbit Satellite | 0.558 ms             | # (167.4 km / 299,792.458) x 1000 = 0.558 ms
<!--
Assuming the use of a low orbit between (167.4 km) and Geostationary orbit (35,786 kilometers)
-->
| Geostationary Satellite   | 119.37 ms            | # (35,786 km / 299,792.458) x 1000 = 119.37 ms
| Earth to the Moon         | 1,275.883 ms         | # (382,500 km / 299,792.458) x 1000 = 1,275.883 ms OR my ping when gaming
<!--
According to Google, the distance varies because of the movement of the earth but is approximately 382,500 km
-->
| Earth to Mars             | 3.035 minutes        | # (54,600,000 km / 299,792.458) x 1000 = 182,125.996 ms / 60,000 ms/min = 3.035 minutes
<!-- 
According to Google, the distance varies because of the movement of our planets but is estimated as approximately 54.6 million kilometers
minute = x / (1000 milliseconds (convert to seconds) x 60 sec)
-->
