
# Código final mejorado

```c
void update_weights(void) 
{
    for (int i = 0; i < num_layers - 1; i++) 
    {
        #pragma omp for
        for (int j = 0; j < num_neurons[i + 1]; j++)
        {
        
          int a = num_neurons[i] * j;
          for (int k = 0; k < num_neurons[i]; k++)  // Update Weights
          {
                  lay[i].out_weights[a + k] = (lay[i].out_weights[a + k]) - (alpha * lay[i].dw[a + k]);
          }
        }
        #pragma omp for  
        for (int j = 0; j < num_neurons[i]; j++)  // Update Bias
            lay[i].bias[j] = lay[i].bias[j] - (alpha * lay[i].dbias[j]);
    }
}
```


---
# Cómo se ha llegado a esto?

### Codigo original
![[Pasted image 20250316184643.png]]

### Perf report
Com s'havia invertit molt temps en millorar `perf_report`, es va decidir millorar `update_weights`.
![[Pasted image 20250316185143.png]]
