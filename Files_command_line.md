#%%%%%%%%%%%%%%%%%%%%%
#Splitting a large results file into smaller ones using command line:

awk 'BEGIN { blockSize=13400; headerSize=5; 
    for (i=1; i<=10; i++) printf("H-bombead -%d SlopeWithsplit.tmp.txt | tail -%d > %s\n",i*(blockSize+headerSize), blockSize, "SlopeWithsplit." i ".txt");}' > splittingScript
source splittingScript


#%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
Select only specific columns to print out of a CSV file (in case we want many fields and want to do the same operation on all of them):

cat myfile.csv | awk -F, 'BEGIN{ORS=""; split("3 4 9 12 123 ”, arr," ")}{for (i in arr) print $i" & " ; print "\\\\ \n"}'


#%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
#%Convert a list of points from a text file to a gMsh .geo file with points:
cat origfile.txt | awk '{print "Point("NR") = {0, "$1", "$2", cl1}; "}' > newfile.geo

#%In case of using an XYZ file and we need the last field, we need to use 
the AWK commands of SPLIT and SUBSTR in order to avoid getting the ^M symbol 
from the end of the line. For example:

cat channel_grid.xyz | awk '{split ($0,arr); print "Point("NR") = {"arr[1]", "arr[2]", " substr(arr[3],1,9)", cl1};" }' > Channel.geo

#%%%%%%%%%%%%%%%%%%
#%Convert a DOS text file to Unix/Mac from Mac command line:
awk '{if(NR==1)sub(/^\xef\xbb\xbf/,"");print}' INFILE > OUTFILE

#%%%%%%%%%%%%%%
#%Mapping a network directory to a local one:
sshfs einatlev@lava.ldeo.columbia.edu://local/data/lava/einatlev/Lakes ~/work/projects/Velocimetry/videos/Lakes/remote/  -oauto_cache,reconnect,defer_permissions,negative_vncache,volname=lava-sshfs,cipher=blowfish

#%%%%%%%%%%%%%%
#String replacement using SED:
sed -i 's/ugly/beautiful/g' /home/bruno/old-friends/sue.txt


#%%%%%%%%%%%%%%%%
#VI, show spaces or tabs:
#first turn on search highlighting (:set hlsearch).

#Show all tabs:
/\t

#Show trailing whitespace:
/\s\+$

#Show trailing whitespace only after some text (ignores empty lines):
/\(\S\+\)\@<=\s\+$

#Show spaces before a tab:
/ \+\ze\t

#%%%%%%%%%%%%%%%%%%%%%%%
#Find which process is using a deviance (e.g. for when amount fails because Device is busy)
lsof <path do device>



#%%%%%%%%%%%%%%%%%%%%%%%%

#Making movies using ffmpeg:
#First step is to change all the names to something that has a serial numbering (file0001.jpg file0002.jpg etc)
#Then run: 
ffmpeg -r 25 -i file%04d.jpg  output.mp4

#%%%%%%%%%%%%%%%%%%
#Network test speed from command line:

git clone https://github.com/sivel/speedtest-cli
cd speedtest-cli 
python2.7 speedtest_cli

(only need to download once)

#%%%%%%%%%%%%%%%%%%%
#Splitting a large text file into chunks:

split -l linenumber filename
(also has other options, e.g. based on file size in bytes)

#%%%%%%%%%%%%%%%%%%%%%%%%%

#%Batch conversion of image formats in Mac:
#%For example, from ,png to .jpg:

for i in *.png; do sips -s format jpeg $i --out Converted/$i.jpg;done
#%%%%%%%%%%%%%%%
#%Batch image cropping in Mac:
#%crop dimension1xdimension2+start pixel x + start pixel y:

for i in *jpg; do convert $i -crop 130x140+250+180 cropped/$i; done

#%%%%%%%%%%%%%%% 
#%combining PDFs form command line on Mac, using a built-in automator script:
#% first time use: 
%cd /usr/local/bin
%sudo ln "/System/Library/Automator/Combine PDF Pages.action/Contents/Resources/join.py" PDFconcat

PDFconcat -o combinedfile.pdf infile1.pdf infile2.pdf infile3.pdf


