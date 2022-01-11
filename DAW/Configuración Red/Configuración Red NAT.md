### Prerrequisitos
[Ubuntu](https://ubuntu.com/download/desktop/thank-you?version=20.04.3&architecture=amd64)

---

## Configurar Red NAT

1.  En virtualbox nos vamos a Archivo > Preferencias > Red > Añadimos una red
    

![](https://lh6.googleusercontent.com/lpU9xSkprjZS0YmtmyF6GQ6t9o0NWtQUvyd83xWmznNrtxsl0zAM4SfIagOtNOtmhnA3x39n3Tjb2qpyroP4SE5Noeu8HzHu3LeJrv5Vp1CCNjH2wBNivo2PNfXl-gbye7reMEx4)

**Red CIDR:** Es _importante_ destacar que estamos hablando de la red asi que debemos terminar con el 0 en vez de un 1 u otro numero. El /24 puedes mirar la explicación en [[Configuración IP estática]]

2.  Dentro de cualquiera de las maquinas virtuales que queremos configurar debemos de entrar a la configuracion y seleccionar Red Nat y el nombre que le hayamos dado a la red.
  ![](https://lh5.googleusercontent.com/zshpzoamO1RrzNoorYDKhGoe74qzdD80yNiZpMRfa1KIdFxfqHsq81cCVQv3XiLd0MDSqd5VIUI4kHby4CIhICKjX_E-RBlhWF1ct4AboViKO_G1DQJ5TPzjSuqDBS0b47fkfaY0)