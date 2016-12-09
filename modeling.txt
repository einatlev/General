%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
Convert a list of points from a text file to a gMsh MSH file:
1. create a triangulation of the points using matlab's delauny function and save as ascii file:
tri = delaunay(x,y);
saveascii (tri, 'delaunymesh.txt', 0);


2. create mesh file:
echo "\$MeshFormat" > newfile.msh
echo "2.2 0 8" >> newfile.msh
echo "\$EndMeshFormat" >> newfile.msh
echo "\$Nodes" >> newfile.msh
wc -l points.txt | awk '{print $1}' >> newfile.msh
cat points.txt | awk '{split ($0,arr); print NR, arr[1], arr[2], substr(arr[3],1,9)}' >> newfile.msh
echo "\$EndNodes" >> newfile.msh
echo "\$Elements" >> newfile.msh
wc -l delaunymesh.txt | awk '{print $1}' >> newfile.msh
cat delaunymesh.txt | awk '{split ($0,arr); print NR, "2 2 0 4", arr[1], arr[2], substr(arr[3],1,13) }' >> newfile.msh
echo "\$EndElements" >> newfile.msh







%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%Going from gMsh .geo file to an Elmer mesh:

gmsh -3 -algo del3d ProfileA.geo -optimize

vi ProfileA.msh
manually delete all elements with type 15 and 
remember to update the number of elements to reflect the ones deleted

ElmerGrid 14 2 ProfileA.msh

%Then edit in ElmerGUI to combine or divide surfaces and edges


%%%%%%%%%%%%%%%%%%
From XYZ point data to PLY/STL:
1. In Matlab (for example), save the point data in a .txt file with fake color and intensity info:
X Y Z 0 0 0 0

2. MeshLab: “import mesh” — the txt file. Select “space” as separator, and the format of the file (X Y Z Ref R G B)

3. Filters->Normals->Compute Normals for points set
4. Filters->Remeshing->Surface reconstruction:Poisson
5. Export mesh as STL or PLY.

http://fabacademy.org/archives/2014/tutorials/pointcloudToSTL.html

