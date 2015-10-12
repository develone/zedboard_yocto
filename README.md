10/12/15 
This requires Xilinx 14.6

Using the dora branch and meta-topic to add snd to the zedboard
The initial image provides the following on core-image-sato
 " kernel-dev fpga-image-adi adi-hdmi-load \
kernel-modules modutils-loadscript zed-audio v4l-utils gsl gsl-dev gnuplot \
chkconfig git jasper  liba52 liba52-dev  \
 libmad libmad-dev libmad-staticdev \
python-netserver   xterm \
python-pygtk python-pygtk-dev python-lxml opencv opencv-samples \
 opencv-apps opencv-dev opencv-samples-dev \
opencv-staticdev python-cheetah x264 x264-dev lua5.1 lua5.1-dev \
lua5.1-staticdev fluidsynth fluidsynth-dev \
 libav libav-dev"

Most of these packages are the depends of vlc
see the file poky_topic.txt
