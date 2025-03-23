#ac 
Practica hecha con [[MobaXterm]]

Nombre del archivo: original.c
Compilado <gcc original.c -Ofast -march=native -o original.c>
	116.162.641.113      cpu_core/cycles:u/
	   108.681.321.108      cpu_core/instructions:u/
	
	      26,549768322 seconds time elapsed
	
	      26,513411000 seconds user
	       0,012972000 seconds sys

versión v2:
Compilado <gcc v2.c -Ofast -march=native -fopenmp -o v2>
Utilizo openMp en la funcion updateBoard:

void __attribute__((noinline)) UpdateBOARD(unsigned IN[], unsigned OUT[], int D, unsigned MAX_VAL) {
  unsigned V[4], tmp;

#pragma omp parallel for private(V, tmp)
for (int x = 1; x < D-1; x++) {
    for (int y = 1; y < D-1; y++) {
        V[0] = IN[x+1+D*y];
        V[1] = IN[x-1+D*y];
        V[2] = IN[x+D*(y-1)];
        V[3] = IN[x+D*(y+1)];

        for (int i = 0; i < 3; i++)
            if (V[i] > V[3]) { tmp = V[i]; V[i] = V[3]; V[3] = tmp; }
        for (int i = 0; i < 2; i++)
            if (V[i] > V[2]) { tmp = V[i]; V[i] = V[2]; V[2] = tmp; }
        for (int i = 0; i < 1; i++)
            if (V[i] > V[1]) { tmp = V[i]; V[i] = V[1]; V[1] = tmp; }

        OUT[x+D*y] = (V[1] + V[2]) % MAX_VAL;
    }
}

}


   706.200.117.663      cpu_core/cycles:u/
   279.953.970.264      cpu_core/instructions:u/

      23,012781351 seconds time elapsed

     176,223608000 seconds user
       0,228980000 seconds sys


  

