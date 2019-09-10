---
published: false
---


-----
# Intro
-----
This was probably one of the more complex Vulnserver exploits that I made, requiring lots of jumping around, stack adjustments, and the infamous alphanumeric character restrictions. I took this as an extra opportunity to practice some manual encoding but also sped things up with [this nice script here](https:github.com/ihack4falafel/Slink/blob/master/Slink.py)

While on one hand I found this more complex than the HP NNM exercise in the CTP course, I simultaniously found it easier, probably just because I was familiar with the concepts at this point, and also knew how to fix a mis-aligned stack (not to mention how to spot it).

By the way, for anyone who may be confused about the new look in the screenshots, I started using [x64dgb](https://x64dbg.com/) because of some of the nicer features here and there.

-----
# Part 1 - Starting with the SEH overwrite
-----

Long-time readers of my original blog should be familiar with the basics of an SEH overwrite, so I'll skip over the finer details and just get right to the important bits.

SEH overwrite occurs when sending 4000 "A"s, and the offsets for SEH and nSEH are around 3500:

![]({{site.baseurl}}/assets/images/lter/01.png)

![]({{site.baseurl}}/assets/images/lter/02.png)

![]({{site.baseurl}}/assets/images/lter/04.png)

![]({{site.baseurl}}/assets/images/lter/05.png)

So right away I've got a basically functioning SEH overwrite in the works. From here on out, the alphanumeric restrictions will start affecting how the exploit is written.

-----
# Part 2 - Alphanumeric shellcoding
-----

So now we get to the interesting part. A jump that doesn't use restricted characters. Remember that alphanumeric means we can only use 0x01 thru 0x7F. The usual JMP SHORT opcode, `\0xEB`, is outside of that range, so we can't use it.

There are a few different ways to go about jumping with alphanumeric characters, but I chose to do it with a conditional jump, `JZ`, or `Jump if 0`. I chose this because I noticed that the ZF flag was set to 1.

![]({{site.baseurl}}/assets/images/lter/06.png)

After taking the jump:

![]({{site.baseurl}}/assets/images/lter/07.png)
