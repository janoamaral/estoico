---
title: (neo)vim autocmd
author: Alejandro Amaral
date: 2021-01-01
hero: ./images/hero.jpg
excerpt: Automatizando procesos con (neo)vim.
---

VIm ([neovim](https://neovim.io/) en realidad) es mi editor de texto/código favorito.
Y hacía un tiempo que no descubría una característica que cambia mi forma de
trabajar. Siempre se dice, cómo una especie de _cliche_, que uno puede usar VIm
durante años y descubrir cosas nuevas. **Y es verdad**.

Haciendo limpieza en los dotfiles (estoy de vacaciones 😎), me encontré con la
molesta tarea de tener que recargar el archivo de configuración de VIm y tumx y
sxhkd y bspwm y... La verdad que era un tedio importante. Hasta que recordé que
había visto en algún lado que VIm traía un comando para realizar acciones cada vez
que se guardaba un archivo. 5 búsquedas en Google más tarde ya sabía lo suficiente
sobre el maravilloso `autocmd`.

[Autocmd](http://vimdoc.sourceforge.net/htmldoc/autocmd.html) permite ejecutar
cualquier acción según un evento. Leer o escribir archivos, buffers, ventanas, etc,
casi todo genera eventos que nosotros podemos explotar. Pero veamos un ejemplo para
entender mejor la documentación anterior

Para recargar automáticamente Neovim al guardar `init.vim` sólo basta con

```
:autocmd! BufWritePost $MYVIMRC source $MYVIMRC | redraw
```

Autocmd define el comando, BufWritePost en el evento que se dispara después de
guardar el archivo $MYVIMRC. Source $MYVIMRC es el comando para volver a cargar el
archivo de configuración. Y por último hacemos un pipe a redraw para que refresque
la pantalla.

Pero esto es la punta del iceberg. ¿Que te parece combinar esto con **comandos de la terminal**?

![I've got the power](https://media1.tenor.com/images/43bf8164532ccfa903b570f9e1889b29/tenor.gif?itemid=4325975)

Cada vez que se edite el archivo de configuración sxhkdrc, reinicia [sxhkd](https://wiki.archlinux.org/index.php/Sxhkd_(Espa%C3%B1ol))
(un hotkey daemon) y manda una notificación de escritorio avisando que fue reiniciado.

```
" Recargar los atajos de teclados al editar sxhkdrc
autocmd! BufWritePost ~/dots/sxhkd/sxhkdrc silent exec "!pkill -USR1 -x sxhkd;notify-send 'sxhkd reloaded'"  | redraw
```

¿Querés más?. ¡Podemos **recompilar programas** al editar el archivo de configuración!

```
" Recompila ST al editar el archivo de configuración
autocmd! BufWritePost ~/dots/st/config.h silent exec "!cd ~/Development/src/st/;make -j2 && make install 2>&1 /dev/null && notify-send 'ST recompilado'" | redraw
```

En este caso recompila la terminal ST cada vez que se cambia el archivo de configuración,
avisandonos con una notificación si la compilación fue exitosa.

### TL;DR.

`autocmd` es un comando que puede tener un **fuerte impacto en la manera que trabajas**. 
Vale muchísimo la pena revisar la documentación, seguro vas a encontrar cosas
para automatizar.
