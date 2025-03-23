

![[Pasted image 20250316160327.png]]

Es va intentar millorar aquesta funció encara més creant una variable auxiliar per tal de fer el sumatori per tal de entrar menys a memòria principal. Tot i així, el nombre total d'encerts variava una mica, probablement degut al canvi de precissió entre operacions.

![[Pasted image 20250316160629.png]]


```c
void update_weights(void) 
{
    for (int i = 0; i < num_layers - 1; i++) 
    {
        #pragma omp for collapse(2)
        for (int j = 0; j < num_neurons[i + 1]; j++)
        {
          for (int k = 0; k < num_neurons[i]; k++)  // Update Weights
          {  
                  lay[i].out_weights[j * num_neurons[i] + k] =
                    (lay[i].out_weights[j * num_neurons[i] + k]) -
                    (alpha * lay[i].dw[j * num_neurons[i] + k]);
          }
          lay[i].bias[j] = lay[i].bias[j] - (alpha * lay[i].dbias[j]);
        }
	    
    }
}
```



----
## Loop unrolling
![[Pasted image 20250316192648.png]]

```c
void forward_prop() {
    for (int i = 1; i < num_layers; i++) 
    {
        #pragma omp for
        for (int j = 0; j < num_neurons[i]; j++) {
            lay[i].z[j] = lay[i].bias[j];
            for (int k = 0; k < num_neurons[i - 1]; k += 4) {
                lay[i].z[j] += (lay[i - 1].out_weights[j * num_neurons[i - 1] + k] * lay[i - 1].actv[k]);
                lay[i].z[j] += (lay[i - 1].out_weights[j * num_neurons[i - 1] + k + 1] * lay[i - 1].actv[k + 1]);
                lay[i].z[j] += (lay[i - 1].out_weights[j * num_neurons[i - 1] + k + 2] * lay[i - 1].actv[k + 2]);
                lay[i].z[j] += (lay[i - 1].out_weights[j * num_neurons[i - 1] + k + 3] * lay[i - 1].actv[k + 3]);
            }


            if (i < num_layers - 1)  // Relu Activation Function for Hidden Layers
                lay[i].actv[j] = ((lay[i].z[j]) < 0) ? 0 : lay[i].z[j];
            else  // Sigmoid Activation Function for Output Layer
                lay[i].actv[j] = 1 / (1 + exp(-lay[i].z[j]));  
        }
    }
}
```

[+] Antes:
![[Pasted image 20250316193337.png]]
![[Pasted image 20250316193406.png]]
[+] 4 Loop Unrolling
![[Pasted image 20250316193054.png]]

![[Pasted image 20250316192622.png]]