#lava flow analysis stuff

%% Plot cross-section of field U, averaged in both time and space:
plot (mean(    mean(Uhs(:,:,x1:x2),3), 1))   %DIM=1 is time, DIM=3 is horizontal

%% Finding edges of flow in image:
bw = rgb2gray (im); %for visible data
%bw = mat2gray (im); % for FLIR data
f = fspecial ('average', [5 5]); %%general 2D filtering!!
bf = imfilter (bw, f);
for i=1:10
    bf = imfilter(bf,f);
end
be = edge (bf, 'canny');
bc = bwmorph (be, 'clean');
bd = bwmorph (bc, 'dilate');

disp ('please pick location of body interiors');
bfh = imfill (bd);
bs = bwmorph (bfh, 'erode');
bff = imfilter(bs,f);
[w,y1,y2] = MeasureChannelWidth(bfh(5:end-5,5:end-5), 300 ,400)

%blob operations:
L = bwlabel (be, 4);
a = regionprops(L, 'basic') %a will hold: area, centroid and bounding box



%%!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

u = data (:,1);
v = data(:,2);
vmag = sqrt(u.^2 + v.^2);
t = data(:,5);
eta = data(:,6);

vmag_grid = griddata (nodes(:,3), nodes(:,4), vmag, X,Y);

p = vmag_grid(:,400)./max(vmag_grid(:,400));


%!!!!!!!!!!!!!!!!!!!!!!!!
%Registering images

I=original image
J = target image
use cpselect to select control points
cpselect(I,JJJ);
Then:
t = cp2tform(input_points1,base_points1,'projective');
[Ireg,xdata, ydata] = imtransform (I,t);
image (Ireg, 'Xdata', xdata, 'Ydata', ydata);
[X,Y] = meshgrid(linspace(xdata(1), xdata(2), size(Ireg,2)), ...
                 linspace(ydata(1), ydata(2), size(Ireg,1)));
TT = interp2(J,X,Y);
pcolore (X,Y,TT);


%!!!!!!!!!!!!!!!
Command history is saved under:
/Users/einatlev/.matlab/R2009b/history.m

%!!!!!!!!!!!!!!!!
%Title for colorbar
h = colorbar;
set (get(h, 'Title'), 'String', 'Temp (C)')


%%!!!!!!!!!!!!!!!!!!
% Adding mapping toolbox to path:

path (path,genpath('/Applications/MATLAB_R2012a.app/toolbox/shared/maputils'))
path (path,genpath('/Applications/MATLAB_R2012a.app/toolbox/map/'))



%%%%%%%%%%%%%%%%%%
Discrete colorbar:

%# create contours with colors indicating 0 and 1
contourf(X,Y,Z,[0 0.5 1])

%# set the colormap to black/white
colormap([0 0 0; 0.5 0.5 0.5; 1 1 1])  %(or use linspace(0,1,length(unique(z)))


%%%%%%%%%%%%%%%%%
%Filtering a time-series using a window pass:

1) Simplest filter: 
data = [1:0.2:4]';
windowSize = 5;
filter(ones(1,windowSize)/windowSize,1,data)

2) a bit more complex:
filt = fir1(20, 0.01); %that's a "one",not and "el"
filtered = filter (filt, 1, mydata);

%%%%%%%%%%%%%%%% Symbolic plotting in matlab
syms x y
ezplot (x^2 + (y - (x^2)^(1/3))^2 - 1) % for example. Gives a heart shape

%%%%%%%%%%%%%%
%Matlab: Create a mask by freehand tracing:
message = sprintf('Left click and hold to begin drawing.\nSimply lift the mouse button to finish');
hFH = imfreehand();
binaryImage = hFH.createMask();




%%%%%%%%%%%%
Rose diagrams that includes magnitudes:
wind_rose (smooth(mod(allangles([1:1900]+i)*180./pi,360),1), smooth(mod(allmags([1:1900]+i),360),1), 'ci', [5:5:35], 'labtitle', ['t=', num2str(hh),':', num2str(mm), ':', num2str(ss)], 'legtype', 0)


%%%%%%%%%%%%%%%%%%%%%%%%
%texture mapping onto a Matlab surface:
h = surf(X,Y,Z);
set(h,'CData',flipud(colortexture),’FaceColor','texturemap')






