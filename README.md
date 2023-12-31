# YoloV8 ile beyin tümör tanıma nasıl yapılır ?
Bu rehberde yolov8 kullanarak object detect uygulaması nasıl yapılacağı anlatılmaktadır. Eğitim işlemi windows işletim sistemi üzerinde yapılacaktır. Bu işlemler için cuda, cudnn driverlarına kullanılarak yapılacaktır.Bu driverların kurulumu için githup hesabıma bakabilirsiniz. Kurulum işlemi hakkında rehber bulunmaktadur.Eğitim anaconda prompt üzerinde koşturulacaktır.
## Adım 1: Dataset Edinme ve Düzenleme
Beyin tümör için dataset drive linkinde bulunmaktadır. Linkten indirip kullabilirsiniz. [Linkte](https://drive.google.com/drive/folders/1D6wuY5n3d5Bmyx1kIfVYnfzfyr_3zqJ2?usp=sharing) resimler ve labellar bulunmaktadır. 
Başka bir nesne ile detect yapmak için kendi datasetinizi toplamanız gerekiyor ve etiketleme işlemi yapmanız gerekiyor. Etiketleme için [makesence ai](https://www.makesense.ai/)  web sitesiniz kullabilirsiniz.
Datasette bulunan resimlerin formatlarına dikkat edin. Tümünün aynı formatta olması gerekmektedir. Yolo için genelde jpg formatı kullanılır. Etiket ve resim düzenlemenin ardında datasetimizi train, test ve validation olmak üzere üçe ayırmalıyız.
Bu işlemi yapan python scriptini yukarı bıraktım. İndirip şu komut ile çalıştırınız:
```bash
python split.py --train 80 --validation 10 --test 10 --folder dataset --dest brain_tumor_dataset
```
![MasterHead](https://github.com/VeliYarar/Brain-Tumor-Detection-with-YOLOv8/blob/main/dataset%20ay%C4%B1rma.png)
Bu komutta bulunan folder kısmı dataseti okucağımız dosyayı veriyoruz dest kısmında ise ayrılmış dataseti kaydedeceğimiz dosya adının veriyoruz.
Datasetimizdeki resimlerin %80 'nini eğitim, %10 'nunu test ve %10 'nu validation olmak üzere üçe bu kod yardımı ile ayırıyoruz.
## Adıım 2: Dosya Düzenleme
C ve D yerel dik konumuna Yolov8 adında dosya oluşturun. Bu dosya içine object_detect adında dosya oluşturun. Bu dosya içine data adında dosya oluşturun. Bu data dosyası içine ayırma işlemi yaptığımız brain_tumor_dataset dosyasını yapıştırın.

## Adım 3: config.yaml Dosyasın Düzenleme

```bash
path: C:\YOLOv8\1_object_detection\data\brain_tumor_dataset

train: images/train # relative to path
val: images/val # relative to path
test: images/test # relative to path

# Class Names
names: ["Tumor"]

```
Örnek bir config dosyası bıraktım.
path datasetimizin bukunduğu dizin; train, test ve val'a ise ayırma işlemi yaptığımız dataset içindeki klasör yolları verilmelidir. names ise class yani detect yaptığımız nesne adıdır.

## Adım 4: Eğitim
Anaconda prompt kullanarak eğitim işlemini gerçekleştireceğiz. Cuda cudnn kurulum işlemi yapılırken [linkte](https://github.com/VeliYarar/CUDA-ve-CuDNN-Windowsa-nasil-kurulur) paylaşınlan adımları izlerseniz sorun yaşamazsınız.

```bash
 conda activate yolov8-gpu
```
Bu komut ile environments değişikliği yapalım. 
cd komutu ile yolo dizinine ardından data dosyasının içinde bulunduğu dizine gelelim.
![MasterHead](https://github.com/VeliYarar/Brain-Tumor-Detection-with-YOLOv8/blob/main/e%C4%9Fitim%20ba%C5%9Flatma.png)


```bash
yolo detect train model=yolov8n.pt data=data/config.yaml imgsz=640 epochs=10 name=yolov8_tumor_detection
```
Bu komut ile eğitimi başlatalım. Komutta bulunan epochs değeri arttıkça eğitim süresi uzar ama kalite artar.
![MasterHead](https://github.com/VeliYarar/Brain-Tumor-Detection-with-YOLOv8/blob/main/e%C4%9Fitim.png)
![MasterHead](https://github.com/VeliYarar/Brain-Tumor-Detection-with-YOLOv8/blob/main/e%C4%9Fitim2.png)

## Adım 4: Prediction
```bash
yolo detect predict model=runs/detect/yolov8_brain_tumor_detect/weights/best.pt source=inference save=True
```
Bu komut ile eğitim sonucunda oluşan modeli test edebiliriz. Eğitim bittiğimnde data dosyasının yanına runs adında dosya oluşacaktır. Model dosyamıza burdan  ulaşabiliriz.
## SONUÇ
![MasterHead](https://github.com/VeliYarar/Brain-Tumor-Detection-with-YOLOv8/blob/main/691.jpg)
![MasterHead](https://github.com/VeliYarar/Brain-Tumor-Detection-with-YOLOv8/blob/main/739.jpg)
![MasterHead](https://github.com/VeliYarar/Brain-Tumor-Detection-with-YOLOv8/blob/main/741.jpg)


