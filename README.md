Download Link: https://assignmentchef.com/product/solved-math3315-project-2-ocean-volume
<br>
Numerical integration of a function of one variable is called quadrature. The analogous situation of integrating a function of two variables is sometimes called <em>cubature</em>. In this project you will explore the extension of the trapezoid formula to this situation, and apply it to find the volume of water in a patch of the ocean.

Consider the problem <sup>RR</sup><em>R f </em>(<em>x</em>, <em>y</em>)<em>dx d y </em>, where <em>R </em>is the rectangle [<em>x</em><sub>0</sub>,<em>x<sub>m</sub></em>]<em>×</em>[<em>y</em><sub>0</sub>, <em>y<sub>n</sub></em>]. We discretize each variable using the space steps <em>h </em>and <em>k</em>, respectively, so that <em>x<sub>i </sub></em>= <em>x</em><sub>0 </sub>+ <em>ih </em>for <em>i </em>= 0,…,<em>n</em>, and <em>y<sub>j </sub></em>= <em>y</em><sub>0 </sub>+ <em>jk </em>for <em>j </em>= 0,…,<em>m</em>. (This requires that <em>h </em>= (<em>x<sub>m </sub>− x</em><sub>0</sub>)<em>/m </em>and <em>k </em>= (<em>y<sub>n </sub>− y</em><sub>0</sub>)<em>/n</em>.) The result is a decomposition of <em>R </em>into rectangles with the nodes at the corners. The strategy is to compute a volume over each little rectangle and sum them over the whole domain. Here’s an example of how the discretization process works in MATLAB:

Construct the domain of integration.

xLeft = 0; xRight = 4; yLeft = -1; yRight = 1; fill([xLeft xRight xRight xLeft],[yLeft yLeft yRight yRight],0.8*[1 1 1]) axis equal, hold on

These are the individual variable discretizations.

<ul>

 <li>= 6; h = (xRight-xLeft)/m;x = xLeft + h*(0:m)’;</li>

 <li>= 5; eta = (yRight-yLeft)/n;y = yLeft + eta*(0:n)’;</li>

</ul>

set(gca,’xtick’,x,’ytick’,y)

2

Now we construct a 2D grid out of them.

[X,Y] = ndgrid(x,y); plot(X,Y,’b.’) plot(X,Y,’k–’)

0.6667    1.3333        2        2.6667    3.3333        4

Both X and Y are (<em>m </em>+1)<em>×</em>(<em>n </em>+1) matrices, and satisfy the identity (<em>x<sub>i</sub></em>, <em>y<sub>j</sub></em>)=(<em>X<sub>i j</sub></em>,<em>Y<sub>i j</sub></em>).

point = [x(3),y(2)] pointAgain = [ X(3,2),Y(3,2) ]

<table width="624">

 <tbody>

  <tr>

   <td width="107">point =1.3333</td>

   <td width="517">-0.6000</td>

  </tr>

  <tr>

   <td width="107">pointAgain =1.3333</td>

   <td width="517">-0.6000</td>

  </tr>

 </tbody>

</table>

At the corners of each rectangle we have values of <em>z </em>according to <em>Z<sub>i j </sub></em>= <em>f </em>(<em>x<sub>i</sub></em>, <em>y<sub>j</sub></em>). We can model the surface <em>z </em>= <em>f </em>(<em>x</em>, <em>y</em>) over one of these rectangles using the function <em>L<sub>i j</sub></em>(<em>x</em>, <em>y</em>)=<em>a </em>+<em>b</em>(<em>x −x<sub>i</sub></em>)+<em>c</em>(<em>y −y<sub>j</sub></em>)+ <em>d</em>(<em>x −x<sub>i</sub></em>)(<em>y −y<sub>j</sub></em>), where <em>a</em>,<em>b</em>,<em>c</em>,<em>d </em>are determined by interpolation conditions at the four corners of the rectangle. (These coefficients depend on <em>i </em>and <em>j</em>, but that is left out to make the notation clearer.) We get a solid defined over the rectangle looking like this:

Here is a single rectangle.

h = 0.1; x = [0 h]’; eta = 0.2; y = [-1 -1+eta]’;

[X,Y] = ndgrid(x,y)

<table width="624">

 <tbody>

  <tr>

   <td width="107">X =0</td>

   <td width="517">0</td>

  </tr>

  <tr>

   <td width="107">0.1000</td>

   <td width="517">0.1000</td>

  </tr>

  <tr>

   <td width="107">Y =-1.0000</td>

   <td width="517">-0.8000</td>

  </tr>

  <tr>

   <td width="107">-1.0000</td>

   <td width="517">-0.8000</td>

  </tr>

 </tbody>

</table>

These are made-up values at the corners of the rectangle.

Z = [ 1.1, 0.9; 0.75, 2 ];

Here are coefficients of the interpolant.

A = [ ones(4,1), X(:), Y(:), X(:).*Y(:) ]; abcd = A  Z(:)

<table width="624">

 <tbody>

  <tr>

   <td width="624">abcd =0.1000 69.0000 -1.000072.5000</td>

  </tr>

 </tbody>

</table>

And here is the resulting surface and the outlines of the solid. (Don’t worry about the fancy plotting commands here.)

ezsurf(@(x,y) abcd(1) + abcd(2)*x + abcd(3)*y + abcd(4)*x.*y, [x;y] ) hold on, shading interp plot3(X,Y,0*Z,’k’,’linew’,2) plot3([1;1]*X(:)’,[1;1]*Y(:)’,[zeros(1,4);Z(:)’],’k’,’linew’,2) axis tight

4

<h1>Objectives</h1>

Mathematical content:

<ol>

 <li>Using a symbolic math package if you want, derive exact expressions for the interpolant <em>L<sub>i j </sub></em>over therectanglewhoselowerleftcorneris(<em>x<sub>i</sub></em>, <em>y<sub>j</sub></em>), intermsof<em>h</em>, <em>k</em>, andfourvalues<em>Z<sub>i j</sub></em>, <em>Z<sub>i</sub></em><sub>+</sub>1<sub>,<em>j</em></sub>, <em>Z<sub>i</sub></em><sub>,<em>j</em>+</sub>1, <em>Z<sub>i</sub></em><sub>+</sub>1<sub>,<em>j</em>+</sub>1. (Show the steps involved, whether you use a computer or not.)</li>

 <li>Integrate the interpolant over the rectangle to get another expression in terms of the same quantities. This is <em>V<sub>i j</sub></em>, the volume used to approximate the integral of <em>f </em>over the rectangle.</li>

 <li>Show that if the <em>V<sub>i j </sub></em>are added up over all <em>i </em>and <em>j</em>, the result is identical to applying the trapezoid formula in each dimension sequentially over the domain (i.e., as an iterated integral).</li>

</ol>

Computational content:

<ol>

 <li>Download and install the GeoMapApp at <a href="http://www.geomapapp.org/">http://www.geomapapp.org</a><a href="http://www.geomapapp.org/">.</a></li>

 <li>Youshouldgetawindowwithaprojectedmapoftheworld, in latitude-longitude corrdinates as measured in degrees of arc, and some GUI widgets. Use the magnifying glass tool to select a small rectangle over the ocean. You should try to get it to include about 1-2 degrees of latitiude (the <em>y </em>axis).</li>

 <li>Click on the icon that looks like a grid. After a few seconds another window should open with a histogram in it. Make sure the dropdown menu at the top of this window says “GMRT Grid” with some version number. Click on the floppy disk icon, select “Grid: .XYZ (ascii format)”, and save the file to your computer. At this point you can close GeoMapApp.</li>

 <li>You can import this data into MATLAB by double-clicking on it in the “Current Folder” tab of the MATLAB Desktop, and using the resulting dialog. What you will get is three columns of <em>x</em>, <em>y </em>, and <em>z </em>data on a regular rectangular grid, where <em>x </em>and <em>y </em>are reported in degrees of arc, and <em>z </em>will be negative to represent depths below sea level.</li>

 <li><strong>Afterconvertingallthedataintokmunits</strong>, use it and the formula you derived above to estimate the amount of water in your patch of ocean.</li>

</ol>

<h1>Submission</h1>

Your submission is composed of two parts: paper submission and online submission.

<h2>online submission</h2>

Have all code included in one runnable .m file. The main routine is written in the form of a function, which calls the function

<h1>function volume = ocean(x,y,z)</h1>

that will compute an approximation to the volume using three vectors of depth data as described in the previous section. Upload the .m file via link on Canvas.

paper submission

Prepare and print a report that includes the following.

<ol>

 <li>The required mathematical content.</li>

 <li>Details needed to acquire the same raw data from the app and compute your volume result.</li>

 <li>Some form of quantitative check on whether your answer is reasonable, in the sense of being at least the right order of magnitude.</li>

</ol>

Your report should read like a document understandable to anyone else in the class. All of the mathematical expressions in your document should be typeset properly as mathematical notation. You have several options: Word, OpenOffice, L<sup>A</sup>T<sub>E</sub>X, texmacs.org, Mathematica. Finalize your report as a single PDF document with the m-file at the end. Print and hand in your report.