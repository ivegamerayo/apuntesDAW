### Prerrequisitos
[Ubuntu 20.04](https://ubuntu.com/download/desktop/thank-you?version=20.04.3&architecture=amd64)
[Configuración Red NAT#Configurar Red NAT](https://github.com/ivegamerayo/apuntesDAW/blob/main/DAW/Configuraci%C3%B3n%20Red/Configuraci%C3%B3n%20Red%20NAT.md)
![[Configuración IP estática#Configuración]]

---

# Servidor Master
## Configuración red

1. Configuracion la [[Configuración Red NAT]] con la ip 

Usando los dos ultimos prerrequisitos configuramos la tarjeta de red con las siguientes caracterisitcas

```
addresses: [192.168.1.124/24]
gateway4: 192.168.1.1
nameservers: [192.168.1.124,8.8.8.8]
```

Si nos fijamos, como DNS estamos utilizando la propia IP del servidor, esto tendrá un fin más adelante.
