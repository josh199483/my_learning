# 在RPI3安裝python3+opencv3
* microSD空間若不太夠可刪除wolfram
>sudo apt-get purge wolfram-engine
* 升級apt
> sudo apt-get update<br>
> sudo apt-get upgrade
* 安裝cmake
>sudo apt-get install build-essential cmake pkg-config
* 安裝圖片檔的函式庫,包括JPEG，PNG，TIFF等
>sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
* 安裝串流影像函式庫
>sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev<br>
>sudo apt-get install libxvidcore-dev libx264-dev
* 安裝GTK函式庫
>sudo apt-get install libgtk2.0-dev
* 安裝OpenCV優化的開發工具
>sudo apt-get install libatlas-base-dev gfortran####
* 安裝 Python 3標頭檔
>sudo apt-get install  python3-dev
* 下載OpenCV的原始檔,這次安裝的是 3.1.0
>cd ~<br>
>wget -O opencv.zip https://github.com/Itseez/opencv/archive/3.1.0.zip <br>
>unzip opencv.zip<br>
* 下載 opencv_contrib 函式庫
>wget -O opencv_contrib.zip https://github.com/Itseez/opencv_contrib/archive/3.1.0.zip<br>
>unzip opencv_contrib.zip
* 安裝 Python 的套件管理程式 pip
>wget https://bootstrap.pypa.io/get-pip.py<br>
>sudo python get-pip.py
* 安裝 Python 虛擬環境 virtualenv和 virtualenvwrapper
>sudo pip install virtualenv virtualenvwrapper<br>
>sudo rm -rf ~/.cache/pip
* 需要更新 ~/.profile 檔案
>nano ~/.profile<br>
>#virtualenv and virtualenvwrapper<br>
>export WORKON_HOME=$HOME/.virtualenvs<br>
>source /usr/local/bin/virtualenvwrapper.sh
* 更新~/.profile
>source ~/.profile
* 創建一個Python的虛擬環境
>mkvirtualenv cv -p python3
* 檢查“CV”虛擬環境
>workon cv
* 安裝  numpy  Python 陣列運算的數學函式函式庫
>pip install numpy
* 安裝過程要在CV虛擬環境中。設置使用CMake的的建構環境
>cd ~/opencv-3.1.0/<br>
>mkdir build<br>
>cd build<br>
```
 cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.1.0/modules \
    -D BUILD_EXAMPLES=ON ..
    (以下make -j4若有錯誤可在加 -D ENABLE_PRECOMPILED_HEADERS=OFF)
```
* 編譯 Opencv   make -j4  是使用4核心
>make -j4
* 安裝OpenCV 3.1.0
>sudo make install<br>
>sudo ldconfig
* 安裝後 OpenCV + Python 應該安裝在 /usr/local/lib/python3.5/site-packages/ 。可用 ls 指令查看
> ls -l /usr/local/lib/python3.5/site-packages/
* 檔名應該是 cv2.so 但是變成cv2.cpython-35m.so 需要手動變更檔案名稱
> cd /usr/local/lib/python3.5/site-packages/<br>
>sudo mv cv2.cpython-35m.so cv2.so
* CV2更名後要聯結Python 3.5的虛擬環境
> cd ~/.virtualenvs/cv/lib/python3.5/site-packages/<br>
>ln -s /usr/local/lib/python3.5/site-packages/cv2.so cv2.so
* 開啟一個新的 SSH 視窗確認OpenCV 安裝正常
>workon cv<br>
>python<br>
```python
import cv2
cv2.__version__
```