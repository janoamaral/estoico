---
title: "De codemonkey a silverback gorilla: remap Caps-Lock"
author: Alejandro Amaral
date: 2021-01-11
hero: ./images/hero.jpg
excerpt: Que el statu quo del teclado te limite... remapear Caps-Lock.
---

Ahhh, otro artículo (y rant) con el _statu quo_ del teclado. Hace un tiempo, hice una 
reseña sobre la [historia de QWERTY y DVORAK](https://logico.ar/blog/2020/01/16/pistolas-maquinas-de-escribir-y-atascos-historia-de-2-teclados).
Ahí expongo algunas razones de diseño de la posición de las teclas en el teclado
QWERTY. Todas (o la mayoría) corresponden a solucionar problemas con las máquinas
de escribir. Problemas que no existen en los teclados de computadoras actuales,
pero por una cuestión de costumbre de uso, ya no se pueden cambiar.

Un caso similar ocurre con las teclas **ESC** y **Caps Lock** (Bloq Mayús). En este
artículo me voy a centrar en Caps-Lock que es el más polémico de los dos 🤬.

![Máquina de escribir](/images/typewriter.jpg)

> Al principio sólo existían máquinas de escribir. Y la gente se acostumbró a
> escribir así. Y nada cambió por los tiempos de los tiempos.

A pesar de ser una sobre simplificación, básicamente la historia es así.

Originalmente las máquinas de escribir tenían una tecla para trabar las mayúsculas
(botón verde en la imagen). En la máquina de escribir estaban las dos juntas porque
una bajaba el carro lo suficiente (shift) para pasar a mayúscula y la otra tecla lo
trababa (caps-lock).

El problema surgió cuando se hicieron los mismos teclados pero para computadoras... Continuaron con la tradición.

Yo, como developer, ~~casi~~ nunca uso esa tecla. Máxime que utilizo
Neovim, que con sólo seleccionar el texto y `U`, ya puedo poner todo el TEXTO QUE
QUIERA EN MAYÚSCULA. Y por si todo eso no fuera suficiente, prácticamente el 99% de
mi trabajo lo realizo en la terminal, tener una tecla muerta en el home row es un pecado.

### Remapeando Caps-Lock a Ctrl en Linux.

¿Por qué Control?. Mis caballitos de batalla son ZSH, Neovim, TMUX y todos ellos
están configurados con `Ctrl` para acceder a las funciones más comunes. La manera
más sencilla de hacer el remapeo es editando el archivo `keyboard` (sudo vi /etc/default/keyboard)
agregando la opción `XKBOPTIONS="ctrl:swapcaps"`.

Mi archivo de configuración:

```bash
# KEYBOARD CONFIGURATION FILE

# Consult the keyboard(5) manual page.

XKBMODEL="pc105"
XKBLAYOUT="es"
XKBVARIANT="dvorak"
XKBOPTIONS="ctrl:swapcaps"

BACKSPACE="guess"
```

En la mayoría de los escritorios (Gnome, KDE, etc) hay una opción en la
configuración del teclado para hacer este cambio de manera gráfica.

![Gnome tweaks](/images/tweaks-tool.png)

Existe también la posibilidad de remapear a cualquier otra tecla. Sólo es necesario 
`xmodmap`. Acá hay un excelente video de cómo hacerlo.

<p>
<iframe width="100" height="500" style="width:100%;height:500px" src="https://www.youtube.com/embed/r3hxmzwwyyE" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</p>

### Remapeando Caps-Lock a Ctrl en Windows.

En Windows, sólo hay que crear una entrada en el registro con `regedit`.

Puedes crear un archivo llamado `Ctrl.reg` con el siguiente contenido

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Keyboard Layout]
"Scancode Map"=hex:00,00,00,00,00,00,00,00,02,00,00,00,1d,00,3a,00,00,00,00,00
```

Y ejecutarlo cómo administrador. O ingresar los siguientes comandos en PowerShell
, también como administrador.

```powershell
$hexified = "00,00,00,00,00,00,00,00,02,00,00,00,1d,00,3a,00,00,00,00,00".Split(',') | % { "0x$_"};

$kbLayout = 'HKLM:\System\CurrentControlSet\Control\Keyboard Layout';

New-ItemProperty -Path $kbLayout -Name "Scancode Map" -PropertyType Binary -Value ([byte[]]$hexified);
```

Reboot y listo.
