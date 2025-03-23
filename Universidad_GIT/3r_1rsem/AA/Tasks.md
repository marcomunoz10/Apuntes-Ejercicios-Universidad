```c#
#pragma omp parallel 
{ 
	#pragma omp single 
	#pragma omp taskgroup 
	{ 
		#pragma omp task 
		compute_update(data1); 
		
		#pragma omp taskloop collapse(2) nogroup 
		for (i=0; i<N; i++) 
			for (j=0; j<M; j++) data2[i,j] = data2[i,j] + 1.3; 
	} // Both data1 and data2 are updated }
}
```
`Collapse(2)` anidar dos bucles en uno.
La cláusula **`nogroup`** en OpenMP proporciona más **flexibilidad** en la ejecución de las tareas porque **evita la sincronización automática** al final del grupo de tareas.
The `taskloop` pragma is used to specify that the iterations of one or more associated loops are executed in parallel using OpenMP tasks.