### Prerrequisitos
- [Ubuntu Desktop](https://ubuntu.com/download/desktop/thank-you?version=20.04.3&architecture=amd64)
- [[Configuración Red NAT]]
---
# Configuración
1. Editamos el archivo ```/etc/netplan/*.yaml``` OJO: Usamos el * ya que depende la versión el nombre cambia.
2. Escribimos la siguiente configuracion
![](https://lh3.googleusercontent.com/JWmQcB-VmSUb42FFVhpd6E--XlV7Ur63a69AwdhZ-OBC3EVm1_CugBxVeJ-QMUz1fuGueu3E3jwZPQH2FJmwFAfqrwNo7iMjhUyk-gxHMW0riFuUKxDLjzXP7JEE66BJM_ebtANNi2BhW3244Q)

### Definición
**enp0s3:**: Es el nombre de la tarjeta de red que queremos configurar.
**dhcp4:** Establecemos _no_ ya que vamos a utilizar estática, si fuese dinámica por [DHCP](https://es.wikipedia.org/wiki/Protocolo_de_configuraci%C3%B3n_din%C3%A1mica_de_host) utilizariamos _yes_ o _true_
**addresses:** Entre corchetes escribimos la *** direccion_ip/mascara_de_red*** en este caso hemos puesto mascara de red 24 ya que estamos usando la mascara*** 255.255.255.0***
**gateway4:** En este caso escribimos la [puerta de enlace](https://es.wikipedia.org/wiki/Puerta_de_enlace)
**nameservers:** Como su nombre indica son los DNS