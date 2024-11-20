# Qué es sys.argv en Python

**sys.argv** es una lista en Python que contiene todos los argumentos de línea de comando pasados al script cuando se ejecuta. Esta lista se utiliza para procesar y analizar los argumentos proporcionados por el usuario al ejecutar el programa.

La lista `sys.argv` se inicializa con el nombre del script como primer elemento (`sys.argv[0]`) y los demás elementos son los argumentos proporcionados en la línea de comando. Por ejemplo, si se ejecuta el script `mi_script.py` con los argumentos `arg1`, `arg2` y `arg3`, la lista `sys.argv` sería:

```
['mi_script.py', 'arg1', 'arg2', 'arg3']
```

Donde `mi_script.py` es el nombre del script y `arg1`, `arg2` y `arg3` son los argumentos proporcionados en la línea de comando.

En Python, `sys.argv` se utiliza comúnmente para:

1. Procesar argumentos de línea de comando para scripts y aplicaciones.
2. Verificar la presencia de argumentos obligatorios o opcionales.
3. Acceder a los valores de los argumentos para utilizarlos en el código.

Es importante destacar que `sys.argv[0]` siempre es el nombre del script, y los demás elementos son los argumentos proporcionados en la línea de comando. Si se invoca el script sin argumentos, `sys.argv` contendrá solo el nombre del script y un valor vacío (`[]`).

En resumen, `sys.argv` es una lista que contiene los argumentos de línea de comando pasados al script, y se utiliza para procesar y analizar dichos argumentos en el código Python.