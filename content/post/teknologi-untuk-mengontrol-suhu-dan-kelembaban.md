---
layout:     post

title:      "Teknologi untuk Mengontrol Suhu dan Kelembaban"
# subtitle:   "Kubernetes webhook扩展机制解析"
description: "Suhu kandang yang tidak sesuai dengan kondisi nyaman dan adanya gas amonia dapat menyebabkan tingginya angka kematian, pertumbuhan yang tidak optimal dan menurunnya produksi telur pada ayam. Tujuan artikel ini adalah untuk menghasilkan model sistem monitoring berbasis elektronis untuk memantau suhu dan gas amonia pada kandang ayam. Model sistem monitoring yang dihasilkan dikembangkan dengan menggunakan komponen elektronika komersial meliputi sensor suhu DHT-1, sensor gas amonia MQ-135, Arduino Pro Mini, Wemos D1, Telegram, dan Blynk. Hasil pengujian pada lingkungan dalam."
excerpt: "Kubernets 1.9版本引入了Admission Webhook(web 回调)扩展机制，通过Webhook,开发者可以非常灵活地对Kubernets API Server的功能进行扩展，在API Server创建资源时对资源进行验证或者修改。 Istio 0.7版本就利用了Kubernets webhook实现了sidecar的自动注入。"
date:       2022-09-16
author:     "Aditya Rieyza"
image: "img/header-image-4.jpeg"
published: true 
tags:
    - Technologi
URL: "/2018/05/23/istio-auto-injection-with-webhook/"
categories: [ Article ]
---

# Teknologi untuk Mengontrol Suhu dan Kelembaban
![screenshot](/img/control-suhu-image.jpeg)
Suhu kandang yang tidak sesuai dengan kondisi nyaman dan adanya gas amonia dapat menyebabkan tingginya angka kematian, pertumbuhan yang tidak optimal dan menurunnya produksi telur pada ayam. Tujuan artikel ini adalah untuk menghasilkan model sistem monitoring berbasis elektronis untuk memantau suhu dan gas amonia pada kandang ayam. Model sistem monitoring yang dihasilkan dikembangkan dengan menggunakan komponen elektronika komersial meliputi sensor suhu DHT-1, sensor gas amonia MQ-135, Arduino Pro Mini, Wemos D1, Telegram, dan Blynk. Hasil pengujian pada lingkungan dalam, luar ruangan dan kandang aktual menunjukkan sistem monitoring mampu mengukur suhu pada rentang 29-33,60oC dan gas amonia pada rentang 0,36-197,56 ppm secara konsisten tanpa ada perubahan hasil pengukuran yang drastis dan tiba-tiba. Sistem yang dihasilkan ini berpotensi untuk diuji pada skala yang luas dan dilakukan uji standar pengukuran.

Secara umum ada dua jenis kandang ayam yaitu kandang ayam sistem terbuka dan kandang ayam sistem tertutup. Kandang ayam sistem terbuka adalah kandang ayam
dimana dinding kandang dibuat ada celah atau bagian seperti jendela atau penutup dinding dari kain atau terpal yang dapat dibuka dan ditutup sewaktu-waktu untuk menjaga suhu kandang pada suhu nyaman. Bagian pada dinding ini atau penutup akan dibuka pada siang hari agar udara dari luar masuk dan akan ditutup pada sore hari untuk menjaga agar suhu dalam kandang tetap hangat.

Sistem elektronis yang diusulkan ini akan mengambil studi kasus untuk kandang sistem terbuka yang banyak digunakan oleh peternakan ayam rakyat yang kebanyakan adalah peternak skala mikro dan kecil di daerah pedesaan. Sistem pemantauan yang dikembangkan dapat digunakan oleh pengelola kandang untuk memantau apakah suhu yang ada dalam kandang sudah sesuai, terlalu panas atau terlalu dingin. Selain itu,
sistem yang diusulkan dapat digunakan untuk me-monitor kadar gas amonia di kandang. Setelah mendapatkan data-data ini kemudian pengelola kandang akan mengambil tindakan yang tepat agar kondisi kandang ayam dapat dijaga pada kondisi nyaman untuk ayam. Sistem yang dikembangkan mengadopsi teknologi Internet of Things (IoT). Keuntungan utama yang diperoleh pada implementasi IoT untuk monitoring variabel di
alam adalah memungkinkan variabel dapat diamati dari jarak jauh secara kontinu dan dalam waktu nyata (Logan, dkk, 2019).
![screenshot](/img/suhu-image-2.png)

Sensor suhu dan sensor gas amonia digunakan untuk mendeteksi dan mengubah suhu dan kandungan gas amonia menjadi sinyal elektronis yang kemudian diolah oleh sistem mikrokontroler yaitu Arduino Pro Mini. Sensor gas amonia yang digunakan adalah sensor komersial MQ-135 berbeda dengan sistem monitoring yang dilaporkan oleh peneliti lain yang menggunakan sensor MQ-137 (Vishesh, dkk, 2016). Besarnya suhu dan kadar gas amonia kemudian ditampilkan pada penampil Liquid Crystal Display (LCD) dan dikirimkan ke mikrokontroler Wemos D1 untuk selanjutnya dikirimkan ke telepon genggam pengelola kandang melalui program aplikasi Telegram dan Blynk.

Aplikasi Telegram dan Blynk dipilih karena mempunyai unjuk kerja yang baik ketika diaplikasikan untuk notifikasi dalam sistem IoT seperti yang juga diaplikasikan untuk sistem keamanan rumah (Kurniawan, dkk, 2018) (Hidayanti, 2020). Sistem elektronis yang dikembangkan juga dilengkapi dengan fitur notifikasi pada aplikasi Telegram untuk memberitahukan apabila suhu atau kadar gas amonia pada kandang melebihi ambang batas yang sudah ditentukan. Jika dibandingkan dengan penggunaan kombinasi antara web dan notifikasi dengan menggunakan pesan pendek Short Message Services (SMS) seperti yang digunakan pada penelitian (Budiarto, dkk, 2020), penggunaan aplikasi Telegram dan Blynk lebih sederhana dalam pengoperasiannya karena hanya membutuhkan data internet saja tanpa perlu tambahan biaya pulsa untuk SMS.

Sensor gas amonia yang digunakan yaitu MQ-135 akan menghasilkan keluaran berupa sinyal analog yang kemudian dikirimkan ke port A0 mikrokontroler Arduino Pro Mini untuk selanjutnya diolah (jalur warna hijau). Sensor suhu yang digunakan yaitu DHT- 11 mempunyai keluaran data digital yang kemudian dikirimkan ke port D2 mikrokontroler Arduino Pro Mini (jalur warna abu-abu). Data temperatur dan gas amonia hasil olahan mikrokontroler Arduino Pro Mini kemudian dikirimkan ke mikrokotroler Wemos D1 untuk selanjutnya dikirimkan ke telepon pintar Android menggunakan aplikasi Telegram dan Blynk. Semua komponen mendapatkan daya dari baterai yaitu VCC yang ditunjukkan pada jalur warna merah sedangkan jalur ground (GND) ditunjukkan pada jalur warna hitam.
![screenshot](/img/suhu-image-3.png)

Untuk dapat bekerja mengolah sinyal maka diperlukan perangkat lunak atau program yang dikembangkan untuk mikrokontroler Arduino Pro Mini dan Wemos D1. Proses komputasi pada mikrokontroler secara keseluruhan dinyatakan dalam diagram alir yang dapat dilihat pada Gambar 3. Tahapan komputasi dimulai dengan melakukan persiapan yaitu inisialisasi sensor yang digunakan yaitu MQ-135 dan DHT-11. Kemudian dilakukan proses kalibrasi pada sensor yaitu antara keluaran sensor dan rentang nilai yang diterima oleh mikrokontroler Arduino Pro Mini.

Setelah mendapatkan data pembacaan sensor, mikrokontroler Arduino Pro Mini melakukan komputasi besarnya suhu dan gas amonia yang terbaca oleh sensor dan kemudian menampilkannya pada penampil LCD dan mengirimkannya ke mikrokontroler Wemos D1. Komputasi pada mikrokontroler Wemos D1 dimulai dengan proses penyambungan Wemos D1 dengan jaringan wifi yang tersedia. Penggunaan jaringan komputer dan internet berbasis nirkabel dengan wifi banyak digunakan dalam pembangunan monitoring suatu variabel jarak jauh seperti yang juga digunakan dalam pemantauan konsumsi energi listrik jarak jauh (Santoso, dkk, 2018). Setelah berhasil tersambung dengan jaringan wifi yang ada maka kemudian Wemos D1 melakukan parsing data suhu dan level gas amonia untuk dikirimkan ke aplikasi Telegram dan Blynk sehingga dapat ditampilkan pada telepon pintar Android. Perangkat lunak atau program pada mikrokontroler dibuat dengan menggunakan Arduino Integrated Development Environment (IDE).
![screenshot](/img/suhu-image-4.png)
<!-- 
## 前言
- - -
Kubernets 1.9版本引入了Admission Webhook(web 回调)扩展机制，通过Webhook,开发者可以非常灵活地对Kubernets API Server的功能进行扩展，在API Server创建资源时对资源进行验证或者修改。

使用webhook的优势是不需要对API Server的源码进行修改和重新编译就可以扩展其功能。插入的逻辑实现为一个独立的web进程，通过参数方式传入到kubernets中，由kubernets在进行自身逻辑处理时对扩展逻辑进行回调。

Istio 0.7版本就利用了Kubernets webhook实现了sidecar的自动注入。

## 什么是Admission
---
Admission是Kubernets中的一个术语，指的是Kubernets API Server资源请求过程中的一个阶段。如下图所示，在API Server接收到资源创建请求时，首先会对请求进行认证和鉴权，然后经过Admission处理，最后再保存到etcd。 
![](/img/2018-4-25-istio-auto-injection-with-webhook/admission-phase.png)
从图中看到，Admission中有两个重要的阶段，Mutation和Validation，这两个阶段中执行的逻辑如下：
* Mutation
  
  Mutation是英文“突变”的意思,从字面上可以知道在Mutation阶段可以对请求内容进行修改。
* Validation

  在Validation阶段不允许修改请求内容，但可以根据请求的内容判断是继续执行该请求还是拒绝该请求。

## Admission webhook
---
通过Admission webhook,可以加入Mutation和Validation两种类型的webhook插件，这些插件和Kubernets提供的预编译的Admission插件具有相同的能力。可以想到的用途包括：
* 修改资源。例如Istio就通过Admin Webhook在Pod资源中增加了Envoy sidecar容器。
* 自定义校验逻辑，例如对资源名称有一些特殊要求。或者对自定义资源的合法性进行校验。

## 采用Webhook自动注入Istio Sidecar
---
### Kubernets版本要求
webhook支持需要Kubernets1.9或者更高的版本,使用下面的命令确认kube-apiserver的Admin webhook功能已启用。

```
kubectl api-versions | grep admissionregistration

admissionregistration.k8s.io/v1beta1
```
### 生成sidecar injection webhook的密钥和证书
Webhook使用数字证书向kube-apiserver进行身份认证，因此需要先使用工具生成密钥对，并向Istio CA申请数字证书。

```
./install/kubernetes/webhook-create-signed-cert.sh /
    --service istio-sidecar-injector /
    --namespace istio-system /
    --secret sidecar-injector-certs
```

### 将生成的数字证书配置到webhook中

```
cat install/kubernetes/istio-sidecar-injector.yaml | /
     ./install/kubernetes/webhook-patch-ca-bundle.sh > /
     install/kubernetes/istio-sidecar-injector-with-ca-bundle.yaml
```

### 创建sidecar injection configmap

```
kubectl apply -f install/kubernetes/istio-sidecar-injector-configmap-release.yaml
```

### 部署sidecar injection webhook

```
kubectl apply -f install/kubernetes/istio-sidecar-injector-with-ca-bundle.yaml
```

通过命令查看部署好的webhook injector

````
kubectl -n istio-system get deployment -listio=sidecar-injector
Copy
NAME                     DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
istio-sidecar-injector   1         1         1            1           1d
```

### 开启需要自动注入sidecar的namespace 

```
kubectl label namespace default istio-injection=enabled

kubectl get namespace -L istio-injection

NAME           STATUS    AGE       ISTIO-INJECTION
default        Active    1h        enabled
istio-system   Active    1h        
kube-public    Active    1h        
kube-system    Active    1h  
```

## 参考

* [Extensible Admission is Beta](https://kubernetes.io/blog/2018/01/extensible-admission-is-beta)
* [Installing the Istio Sidecar](https://istio.io/docs/setup/kubernetes/sidecar-injection.html) -->
