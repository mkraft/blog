---
title: Binary Search for Kids
---

Here's an easy way to introduce kids to thinking algorithmically. It allows them to intuitively use **binary search** using a whiteboard grid to visually represent some elements. 

Everything until Advanced Lessons is appropriate for kids about 5 years old and up.

### Method

1. Draw 64 dots on a whiteboard in an 8 x 8 grid.
2. Chose a dot; don't share your choice. 
3. Ask the kids to figure out which dot you've chosen.
4. Track their guess counts and have them try to improve their score.

![8x8 grid](/assets/images/step-0.jpeg)

They may start by guessing each dot one at a time (a **linear/sequential search**). You can get really lucky and get it in 1 guess, but it's not likely. They'll likely come up with their own algorithm like asking about each row one at a time and then doing a sequential search along the row, or something like that. However, you can show them a way that is consistent at these small number and **scales** much, much better for larger sets of elements: binary search!

### Example

Say I've selected a dot. First you would draw a line through the middle and ask "is it on the left?" "No," I would tell you.

![8x8 grid](/assets/images/step-1.jpeg)

Then—focussing on the right side only—you'd draw a line through the middle of that side. "Is it on the top?" "Yes," I would say.

![8x8 grid](/assets/images/step-2.jpeg)

We'd repeat the pattern **recursively** until the choice is unequivocal.

![8x8 grid](/assets/images/step-3.jpeg)
![8x8 grid](/assets/images/step-4.jpeg)
![8x8 grid](/assets/images/step-5.jpeg)
![8x8 grid](/assets/images/step-6.jpeg)

In just 6 guesses (the number of lines you drew), you correctly derived that I was thinking of the dot at position (5, 5).

### Advanced Lessons

You can get deep into mathematics and computer science, but here's a starting point:

* **Binary logarithm** for deriving the worst case performance ⌊log2(n) + 1⌋. Why is there an extra 1 added? If the element being searched for is not present we still need to check it, so we include that check in the cost.
* The **slow growth** relationship between the number of the elements and performance. Searching 64 elements has a cost of 7 because ⌊log2(64) + 1⌋ = 7, etc. Searching 1 billion elements takes only 30 guesses. Imagine the time to search 1 billion elements sequentially!
* **Data structures** like **sorted arrays** and **binary search trees**.

### Conclusion

My kids loved this. My 8-year-old was perfectly executing the algorithm and my 5-year-old woke up the next morning and asked "can we play find the dot?" Her algorithm is yet to be proven :)
