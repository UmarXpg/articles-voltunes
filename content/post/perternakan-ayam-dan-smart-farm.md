---
layout:     post

title:      "Peternakan Ayam dan Smart Farm"
# subtitle:   "来自Istio的儿童节礼物"
excerpt: "Istio 0.8 Release新特性"
date:       2022-09-17T18:00:00
author:     "Voltunes"
description: "Daging ayam merupakan salah satu sumber makanan hewani yang memiliki permintaan tinggi di Indonesia.Berdasarkan data yang dikeluarkan BPS pada tahun 2021, daging ayam memiliki tingkat konsumsi sebesar 8,1 kg per kapita. Nilai tersebut menempati urutan pertama tingkat konsumsi masyarakat Indonesia dibandingkan dengan konsumsi daging sapi sebesar 2,2 kg per kapita, daging domba sebesar 0,4 kg per kapita, dan daging babi sebesar 1 kg per kapita. Angka tersebut sebanding dengan jumlah persediaan ayam pedaging yang berada pada angka 3.107.183.054 ekor (per tahun 2021)."
image: "img/image-header-3.jpeg"
published: true 
tags:
    - Smart Farm 

categories: [ Insight ]
URL: "/2018/06/02/istio08/"
---

# Peternakan Ayam dan Smart Farm
![screenshot](/img/ayam2.png)
Daging ayam merupakan salah satu sumber makanan hewani yang memiliki permintaan tinggi di Indonesia. Berdasarkan data yang dikeluarkan BPS pada tahun 2021, daging ayam memiliki tingkat konsumsi sebesar 8,1 kg per kapita. Nilai tersebut menempati urutan pertama tingkat konsumsi masyarakat Indonesia dibandingkan dengan konsumsi daging sapi sebesar 2,2 kg per kapita, daging domba sebesar 0,4 kg per kapita, dan daging babi sebesar 1 kg per kapita. Angka tersebut sebanding dengan jumlah persediaan ayam pedaging yang berada pada angka 3.107.183.054 ekor (per tahun 2021).

Seiring dengan jumlah permintaan yang terus naik, banyak perusahaan bersaing untuk mendorong kapasitas produksinya untuk terus meningkat. Beberapa nama seperti Charoen Pokphand Group, Japfa Comfeed Indonesia, dan Indojaya Agrinusa adalah supplier utama dari tersedianya ayam potong di Indonesia. Namun demikian juga tidak sedikit usaha mandiri ataupun koperasi yang didirikan oleh perseorangan dalam melakukan ternak ayam broiler.

Ayam broiler sendiri merupakan jenis ayam yang memiliki laju perkembangan dan hasil ternak berkualitas. Jenis ayam broiler memiliki usia pertumbuhan antara 30-35 hari dengan berat 2-3 kg. Ayam ini tergolong dapat diternakkan dimana saja baik dengan jenis kandang terbuka atau tertutup. Walaupun tergolong jenis unggul, peternakan ayam broiler kerap mengalami beberapa permasalahan baik terkait kondisi kandang maupun kesehatan dari ayam sendiri.

Permasalahan terkait kandang yang sering dialami oleh peternakan meliputi sirkulasi udara tidak bagus, pemberian pakan dan minum yang salah, dan intensitas cahaya tidak merata. Beberapa masalah tersebut dapat mempengaruhi kondisi kesehatan ayam seperti dengan timbulnya penyakit gumboro (infeksi pencernaan), tetelo (masalah pernafasan), diare kapur, penyakit ngorok, dan flu burung. Adapun selain mudah menyebar beberapa penyakit tersebut dapat menyebabkan kematian dalam satu kawanan ayam.

Guna mengatasi serangkaian masalah yang timbul dalam kandang, kini banyak perusahaan melakukan penerapan teknologi berupa sensor dan mesin yang terotomatisasi. Pemasangan teknologi memberikan kondisi kandang yang lebih terkontrol dan dapat diukur dengan lebih riil. Adapun selain pemberian pakan dan minum secara otomatis, terdapat beberapa sensor yang mampu memberikan informasi terkait suhu, kadar udara, intensitas cahaya, dan berat ayam. Informasi yang telah dikumpulkan sensor kemudian dapat dijadikan dasar dalam menentukan tindakan yang sebaiknya diambil.

Melalui penerapan teknologi smart farm, kapasitas dan kualitas produksi dapat ditingkatkan menjadi lebih baik. Kualitas hidup ayam akan menjadi lebih baik dan tingkat kematian ayam akan jauh lebih berkurang. Selain itu dengan data yang terukur secara riil dapat memberikan dasar pengambilan keputusan yang lebih spesifik dan tepat sasaran.
<!-- 
> 在6月1日这一天的早上，Istio社区宣布发布0.8 Release，除了常规的故障修复和性能改进外，这个儿童节礼物里面还有什么值得期待内容呢？让我们来看一看：
## Networking

### 改进的流量管理模型
0.8版本采用了新的流量管理配置模型[v1alpha3 Route API](https://istio.io/blog/2018/v1alpha3-routing/)。新版本的模型添加了一些新的特性，并改善了之前版本模型中的可用性问题。主要的改动包括：

#### Gateway
新版本中不再使用K8s中的Ingress，转而采用Gateway来统一配置Service Mesh中的各个HTTP/TCP负载均衡器。Gateway可以是处理入口流量的Ingress Gateway，负责Service Mesh内部各个服务间通信的Sidecar Proxy，也可以是负责出口流量的Egress Gateway。

Mesh中涉及到的三类Gateway:    
![Gateway](/img/2018-06-02-istio08/gateways.svg)    

该变化的原因是K8s中的Ingress对象功能过于简单，不能满足Istio灵活的路由规则需求。在0.8版本中，L4-L6的配置和L7的配置被分别处理，Gateway中只配置L4-L6的功能，例如暴露的端口，TLS设置。然后用户可以采用VirtualService来配置标准的Istio规则，并和Gateway进行绑定。


#### VirtualService

采用VirtualService代替了alpha2模型中的RouteRule。采用VirtualService有两个优势：

**可以把一个服务相关的规则放在一起管理**

例如下面的路由规则，发向reviews的请求流量缺省destination为v1，如果user为jason则路由到v2。在v1模型中需要采用两条规则来实现，采用VirtualService后放到一个规则下就可以实现。
```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
    - reviews
  http:
  - match:
    - headers:
        cookie:
          regex: "^(.*?;)?(user=jason)(;.*)?$"
    route:
    - destination:
        host: reviews
        subset: v2
  - route:
    - destination:
        host: reviews
        subset: v1
```

**可以对外暴露一个并不存在的“虚拟服务”，然后将该“虚拟服务”映射到Istio中的Service上**

下面规则中的bookinfo.com是对外暴露的“虚拟服务”，bookinfo.com/reviews被映射到了reviews服务，bookinfo.com/ratings被映射到了ratings服务。通过采用VirtualService，极大地增强了Istio路由规则的灵活性，有利于Legacy系统和Istio的集成。
```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: bookinfo
spec:
  hosts:
    - bookinfo.com
  http:
  - match:
    - uri:
        prefix: /reviews
    route:
    - destination:
        host: reviews
  - match:
    - uri:
        prefix: /ratings
    route:
    - destination:
        host: ratings
  ...
```

### Envoy V2
控制面和数据面标准接口支持Envoy 

### 用Gateway代替 Ingress/Engress
前面已经介绍到，新的版本中不再支持将Kubernetes的Ingress和Istio路由规则一起使用。Istio 0.8支持平台无关的 Ingress/Egress Gateway,可以在Kubernetes，Cloud Foundry中和Istio路由规则无缝集成。

### 对入站端口进行限制
0.8版本只允许访问Pod内已声明端口的入站流量。

## Security
### 安全组件Citadel
将Istio的安全组件Istio-Auth/Istio-CA正式命名为Citadel（堡垒）。

### 跨集群支持
部署在多个Cluster中的Citadel可以共享同一Root Certificate，以支持不同Cluster内的服务可以跨Cluster进行认证。

### 认证策略 
认证策略既支持Service-to-Service认证，也支持对终端用户进行认证。

## 遥测
Mixer和Pilot将上报自身的遥测数据，其上报的流程和Mesh中的普通服务相同。

## 安装
按需安装部分组件：支持只安装所需的组件，如果只需要使用Istio的路由规则，可以选择只安装Pilot，而不安装Mixer和Citadel。

## Mixer
CloudWatch：增加了一个CloudWatch插件，可以向AWS CloudWatch上报度量数据。

## 已知故障：
* 如果Gateway绑定的VirtualService指向的是headless service，则该规则不能正常工作。
* 0.8版本和Kubernetes1.10.2存在兼容问题，目前建议采用1.9版本。
* convert-networking-config工具存在故障，一个其它的namespace可能会被修改为istio-system namespace。可以在允许转换工具后手动修改文件来避免。

## 总结
0.8版本带来的最大变化是流量配置模型的重构，重构后的模型整合了外部Gateway和内部Sidecar Proxy的路由配置。同时VirtualService的引入使路由规则的配置更为集中和灵活。 -->
