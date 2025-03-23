
Al poner un `#pragma omp` solo se aplica la directiva al bucle en cuestión. Es decir, si existiera un bucle dentro, éste actuaría en secuencial.

Ejemplo:

```C
#include <omp.h>
#include <cmath>

void forward_prop() {
    for (int i = 1; i < num_layers; i++) {
        #pragma omp parallel for
        for (int j = 0; j < num_neurons[i]; j++) {
            lay[i].z[j] = lay[i].bias[j];
            for (int k = 0; k < num_neurons[i - 1]; k++) {
                lay[i].z[j] +=
                    ((lay[i - 1].out_weights[j * num_neurons[i - 1] + k]) *
                     (lay[i - 1].actv[k]));
            }

        }
    }
}
```