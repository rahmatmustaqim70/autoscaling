# Contoh Implementasi Horizontal Scaling pada kubernetes

1. Clone github ini. lalu masuk ke dalam direktori autoscaling.
2. Gunakan workspace katacoda : https://katacoda.com/athertahir/scenarios/kubernetes-for-beginners-minikube
3. Jalankan minikube dengan perintah : minikube start
4. Buatlah docker image dengan nama php-apache : docker build -t php-apache .

## AutoScaling
Kemudian jalankan container dalam kubernetes dengan perintah:

$ kubectl run php-apache --image=k8s.gcr.io/hpa-example --requests=cpu=200m --expose --port=80

[output]
service/php-apache created
deployment.apps/php-apache created

Setelah server berjalan, kita akan membuat autoscaler menggunakan perintah "kubectl autoscale". Perintah berikut ini akan membuat Autoscaler Pod Horisontal untuk mengelola 1 - 10 replikasi Pod yang untuk mengendalikan php-apache deployment. Secara umum, HPA akan menambah atau mengurangi jumlah replika (melalui deployment) untuk memjaga utilitas CPU agar tetap memiliki beban rata-rata Pods 50% (dalam contoh ini setiap pod meminta 200 mili-core oleh kubectl run, ini berarti rata-rata penggunaan CPU 100 mili -cores)

$ kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=10

$ kubectl get hpa

##Load testing

$ kubectl run -i --tty load-generator --image=busybox /bin/sh

Hit enter for command prompt

$ while true; do wget -q -O- http://php-apache.default.svc.cluster.local; done

$ kubectl get hpa



