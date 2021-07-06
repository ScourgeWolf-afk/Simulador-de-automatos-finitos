# Simulador-de-automatos-finitos
Implementaçao

#include <stdio.h> //biblioteca entrada de dados
#include <string.h> //para strings...
#include <stdlib.h> //alocacao dinamica...


struct dados{
    char alfabeto[50];
    int *estadofinal;
    int *n_estados;
    int estadoInicial;
    struct dados **matrizTransicao;
};


void lerArquivo ( const char *diretorio){

    FILE *f_arquivo ;

    struct dados d;

    f_arquivo = fopen(diretorio,"r");

    if (!f_arquivo){
        fprintf(stdout,"\n Nome do arquivo ou diretorio errado: %s\n",diretorio);
        exit(-1); //encerre imediatamente...
    }

    //primeira linha estado inicial;
    fscanf(f_arquivo,"%s %d",d.alfabeto,&d.estadoInicial);

    char c;
    printf("\n============================================\n");

    while ((c = fgetc(f_arquivo)) != EOF){
        if (c == ','){
            fscanf(f_arquivo,"%d",&d.n_estados);
        }else if (c == '$'){
            printf("%c elemento vazio",c);
        }
        printf("%c ",c);
    }

    fscanf(f_arquivo,"%d",d.estadofinal);

    printf("\n=============================================\n");

    fprintf(stdout,
                "\nAlfabeto: %s"
                "\nEstadoInicial: %d"
                "\nNumero de estados: %d"
                "\nEstado Final: %d",
                d.alfabeto,
                d.estadoInicial,
                d.n_estados,
                d.estadofinal);
    printf("\n==============================================\n");

    int tamanho = d.n_estados;
    d.matrizTransicao = (struct dados **) malloc(sizeof(struct dados)*(tamanho));
    int i;

    for (i=0; i <tamanho; i+=1){
        d.matrizTransicao[i] = (struct dados **) malloc(strlen(d.alfabeto) *sizeof(int));
    }

    int x,y,j;

    printf("\n\nFuncao de Transicao do A.F.D\n\n");

    printf("================================\n");

    int tamanhoPalavra = strlen(d.alfabeto);

    for(i=0; i<tamanho; i++){
	    for(j=0; j< tamanhoPalavra; j++){

		if ( fscanf(f_arquivo, "%i %c %i", &x, &y, &d.matrizTransicao[i][j]) != 3){
            exit(-1);
		}
		printf("|  Q%i  |   %c   |   Q%i   |\n", x, y, d.matrizTransicao[i][j]);
		printf("-------------------------\n");
	   }
    }


    fclose(f_arquivo);


}

int  validaPalavra(struct dados d,char *simbolo ){
    int i;
    int estado;
    int **t = NULL;
    int *finais = NULL;

    t = d.matrizTransicao;
    estado = d.estadoInicial;
    finais = d.estadofinal;

    for (i=0; simbolo[i] != '\0'; i+=1 ){
        estado = t[estado][simbolo[i] -'a'];
        printf("\n %c -> Q:%d \n",simbolo[i],estado);

    }

    if (finais[estado] == estado){
        printf("\n Palavra foi aceita: %s o estado de parada foi Q: %d\n",simbolo,estado);
        return 1;
    }else{
	    printf("\n\n\nPalavra ( %s ) foi REJEITADA! O estado de parada foi: Q%i\n\n\n", simbolo, estado);
	    return 0;
    }


}


int main(int argc,char **argv){

    printf("\n===========================================\n");
    printf("\n      SIMULADOR DE AUTOMATOS FINITOS       \n");
    printf("\n===========================================\n");

    char palavra[30];
    char nome_arq[50];
    char c;
    int parar = 0;
    struct dados d1;

    while (!parar ){

        printf("\nInforme o nome do arquivo: ");
        scanf("%s",nome_arq);

        system("cls");
        lerArquivo(nome_arq);

        printf("\n Palavra: ");
        scanf("%s",palavra);

        validaPalavra(d1,palavra);

        do {

            printf("\n Deseja abrir outro arquivo [S/N]: ");
            scanf("%c",&c);

            if (c == 'S'){
                parar = 0;
            }else if (c == 'N'){
                parar = 1;
            }

        }while (c != 'S' || c != 'N');



    }

    system("pause");

    return 0;


}
