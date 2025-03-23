
Error:
```c++
floatreduce_add (float* In,int N) 
{
	float sum= 0.0, Out;
	#pragma omp parallelnum_threads(4)\
	shared(In,Out,N) firstprivate(sum)
	{
		#pragma omp for 
		for ( inti=0; i<N; i++)
			sum += In[i];
		Out+= sum; 
		}
		returnOut;
}
```

Puede ocasionar condición de carrera.

Solución:

```c++
floatreduce_add (float* In,int N) 
{
	float sum= 0.0, Out;
	#pragma omp parallelnum_threads(4)\
	shared(In,Out,N) firstprivate(sum)
	{
		#pragma omp for 
		for ( inti=0; i<N; i++)
			sum += In[i];
		#pragma omp atomic
		Out+= sum; 
		}
		returnOut;
}
```

Solución óptima:
```c++
floatreduce_add (float* In,int N) 
{
	float Out;
		#pragma omp parallel for num_threads(4) \ 
			shared(In,N)reduction(+:Out) 
	for ( int i=0; i<N; i++)
		Out+= In[i]; 
	returnOut; 
}
```