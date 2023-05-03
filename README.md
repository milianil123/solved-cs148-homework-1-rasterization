Download Link: https://assignmentchef.com/product/solved-cs148-homework-1-rasterization
<br>
Introduction to Computer Graphics and Imaging

This is the first homework assignment for CS 148. We will have weekly assignments except when there are exams; check the course website for due dates, assignment materials, and the late day policy. Assignments like this are half written and half coding.

Note that algorithms like the one we are discussing in this assignment are wellknown and probably easy to find online; copying code or math from any such source is <em>strictly forbidden</em>.

In Lecture 2, we derived Bresenham’s algorithm for rasterizing lines. In this assignment, we are going to derive and implement a similar algorithm for rasterizing circles. In particular, we will rasterize the quadrant of the circle <em>x</em><sup>2</sup>+ <em>y</em><sup>2 </sup>= <em>R</em><sup>2 </sup>where <em>x</em>, <em>y </em>≥ 0 and positive integer <em>R</em>.

One obvious way you could attempt to rasterize such a circle would be to iterate over each<sub>√        </sub><em>x</em>

corresponding to a pixel center and light up the pixel closest to (<em>x</em>,                  <em>R</em><sup>2</sup>− <em>x</em><sup>2</sup>).

<strong>Problem 1 </strong>(10 points)<strong>. </strong><em>Why is this approach bad? Provide at least two reasons.</em>

Instead, we will imitate our development of Bresenham’s algorithm for rasterizing lines. In fact, we can simplify matters somewhat: We can employ the symmetry of the circle to reduce the number of pixels we have to rasterize. Then, each time we rasterize a pixel we will also light up its symmetric counterpart.

<strong>Problem 2 </strong>(10 points)<strong>. </strong><em>Suppose we are given </em>(<em>x</em>, <em>y</em>) <em>on the </em>45<sup>◦ </sup><em>arc of the circle between </em>(0, <em>R</em>) <em>and</em>

(<em>R</em>/√2, <em>R</em>/√2)<em>. Find a corresponding symmetric point on the </em>45<sup>◦ </sup><em>arc from </em>(<em>R</em>/<sup>√</sup>2, <em>R</em>/<sup>√</sup>2) <em>to </em>(<em>R</em>, 0) <em>that takes no additional operations to compute.</em>

For deriving methods like Bresenham’s algorithm, it is often more convenient to use <em>implicit </em>forms of curves. So, we define <em>F</em>(<em>x</em>, <em>y</em>) = <em>x</em><sup>2 </sup>+ <em>y</em><sup>2 </sup>− <em>R</em><sup>2</sup>. Then, our curve is given by <em>F</em>(<em>x</em>, <em>y</em>) = 0. Note that <em>F</em>(<em>x</em>, <em>y</em>) <em>&lt; </em>0 when (<em>x</em>, <em>y</em>) is inside the circle and <em>F</em>(<em>x</em>, <em>y</em>) <em>&gt; </em>0 when (<em>x</em>, <em>y</em>) is outside the circle.

Just as in our method for rasterizing lines, we will be marching from left to right. Let’s say we are at a pixel whose center is (<em>x</em>, <em>y</em>). Then, neighboring pixel centers are given by (<em>x </em>± 1, <em>y </em>± 1). Our algorithm will make a local decision about which neighbor to choose next in its walk around the circle.

<strong>Problem 3 </strong>(10 points)<strong>. </strong><em>Show that the slope of the circle in the </em>45<sup>◦ </sup><em>arc we are drawing satisfies </em>−1 ≤ <em>dy</em>/<em>dx </em>≤ 0<em>. Use this fact to justify why if we trace pixels in increasing x along the circle, it is sufficient to consider only the neighbors </em>(<em>x </em>+ 1, <em>y</em>) <em>and </em>(<em>x </em>+ 1, <em>y </em>− 1) <em>of </em>(<em>x</em>, <em>y</em>)<em>.</em>

1

Suppose we just drew the pixel at (<em>x<sub>P</sub></em>, <em>y<sub>P</sub></em>). Our method will have the property that the circle will intersect the segment between (<em>x<sub>P</sub></em>, <em>y<sub>P </sub></em>−1/2) and (<em>x<sub>P</sub></em>, <em>y<sub>P </sub></em>+1/2) (it may help if you draw this for yourself). We will define our <em>decision variable </em>to be <em>d </em>= <em>F</em>(<em>x<sub>P </sub></em>+ 1, <em>y<sub>P </sub></em>−<sup>1</sup>/2).

<strong>Problem 4 </strong>(10 points)<strong>. </strong><em>Give a rule using d for choosing between </em>(<em>x<sub>P </sub></em>+ 1, <em>y<sub>P</sub></em>) <em>and </em>(<em>x<sub>P </sub></em>+ 1, <em>y<sub>P </sub></em>− 1) <em>using the property explained above.</em>

We now have a rule for choosing which pixel to visit next, but now we’ll need to update <em>d</em>.

We will call the move from (<em>x<sub>P</sub></em>, <em>y<sub>P</sub></em>) to (<em>x<sub>P </sub></em>+ 1, <em>y<sub>P</sub></em>) a move <em>east </em>(E) and the move from (<em>x<sub>P</sub></em>, <em>y<sub>P</sub></em>) to (<em>x<sub>P </sub></em>+ 1, <em>y<sub>P </sub></em>− 1) a move <em>southeast </em>(SE).

<strong>Problem 5 </strong>(10 points)<strong>. </strong><em>Provide update rules of the form d<sub>new </sub></em>= <em>d<sub>old </sub></em>+∆<em><sub>E </sub>and d<sub>new </sub></em>= <em>d<sub>old </sub></em>+∆<em><sub>SE </sub>to be applied depending on which pixel you choose.</em>

Excitingly, you should find that if <em>x<sub>P</sub></em>, <em>y<sub>P </sub></em>are integers, so are ∆<em><sub>E </sub></em>and ∆<em><sub>SE</sub></em>. This suggests that we can do integer arithmetic while rasterizing circles, which can buy us some time!

To complete our rasterization algorithm, we just need a way to start out. For simplicity, we’ll actually change the convention we explained in class and assume <em>pixel centers coincide with integer coordinates</em>. So, we’ll start at (<em>x</em>, <em>y</em>) = (0, <em>R</em>).

<strong>Problem 6 </strong>(10 points)<strong>. </strong><em>Provide an initial value for d and show that it isn’t an integer. Suggest an alternative decision variable h of the form h </em>= <em>d </em>− <em>c for some fixed constant c so that the initial value of h is an integer, and provide update and decision rules for h.</em>

This completes our derivation of the Bresenham algorithm for rendering circles. To make sure you have all the pieces organized, you will implement it in C++.

<strong>Problem 7 </strong>(40 points)<strong>. </strong><em>Implement your circle rasterization algorithm in C++. Skeleton code is provided on the CS 148 course website, including guidance on input and output. You must use </em>integer arithemetic <em>in your code – programs using </em>double<em>s or any other type of decimal arithmetic will receive no credit.</em>

2