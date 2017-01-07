---
layout: post
title:  "On solving a problem"
date:   2015-10-29 10:00:00
categories: code problem
---


# On solving a problem:

I thought of a problem today. If you have a sorted deck of cards, and are able to perfectly rifle shuffle it, how many shuffles do you need until the deck's back in sorted order.[*]

### fun right? 

Intuitevly, I felt like the answer should be like 26 or 52, no good reason, just a gut feeling. Maybe some sort of power of 2 thing, since the shuffle really reeks of 'power of 2' type work.

And now comes the wrinkle. I've never been the type of person to look in the back of the book for the answer, I usually let a problem fester in my head. And to me, there's no greater feeling than figuring out the answer, and being so sure of the process that you don't need to look at the back of the book.

So... I thought it'd be really easy to program the problem, and just simulate it, and see how many times it'd take. So I did that in JavaScript. I mean, it's fun to write code, it's a challenge, it's not cheating, right?!

## **Spoilers**
The answer is 8. It takes 8 times. That's what my code says, now I need to justify it. To do the penance for my sin. Well, I tried printing out some logs, and following the order of the cards, and I thought I'd just track one card (the 2nd card in the deck).

Shuffle | Card 2 position (zero indexing)
--- | ---
0 | 1
1 | 2
2 | 4
3 | 8
4 | 16
5 | 32
6 | 13 <--- what?
7 | 26
8 | 1

Alright, we've got another wrinkle. It makes sense that before the final shuffle that orders everything, one deck is all odd and one is all even, so the 2nd card is in position 26. But what's with that 13? (not a power of 2, I'm almost POSITIVE).

i'd expect

Shuffle | Card 2 position
--- | ---
6 | 64
7 | 128
8 | 256

But there aren't that many cards in the deck, of course! modulus!

64%52 = 12... that's not right. Oh, maybe you don't count the card itself, so only 51, zero-indexing... 

64%51 = 13, perfect.

128%51 = 26

256%51 = 1

Alright, penance paid. Forgive me Martin Gardner.

So, what have I learned for the general case... a card gets back to its original position in x shuffles, according ot the equation:

(2^x)%(n-1) = 1

`x` is the number of shuffles
`n` is the number of cards 

Wow... I need to rearange that. can I even have a modulus in an equation...


[*]in the shuffle, you always shuffle the top half onto the bottom half, so the top card is always on top, and the bottom card is always on bottom.