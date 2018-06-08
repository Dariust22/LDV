# LDV
Programa de locação e gerenciamento de veículos
#include <stdio.h>
#include <stdlib.h>
#include <conio.h>
#include <conio2.h>
#include <string.h>
#include <locale.h>
#include <windows.h>

struct cadastro_cliente{
 int ativo;
 char nome[100],rg[16],cpf[16],nasc[20],rua[100],bairro[100],ncasa[10],cidade[20],estado[20],cep[10],telefone[14];
 };
 struct cadastro_veiculo{//: Modelo, Ano, Placa, Cor;
     int ativo2;
char marca[20],modelo[30],ano[20],placa[20],cor[20];
};
struct juncion{
 char nome[50],rg[16],cpf[16],nasc[20],rua[100],ncasa[10],bairro[100],cidade[20],estado[20],cep[10],telefone[14];
char marca[20],modelo[30],ano[20],placa[20],cor[20];
char datatual[20],datdev[20];
char qtd[20], preco[20],ptotal[20];
int ativo3;

};


char usuario[20]="usuario";
char senha[20]="123456";

typedef struct cadastro_cliente cli;
typedef struct cadastro_veiculo vei;
typedef struct juncion jun;


jun j;
cli c;
vei v;
//--------------------------------------------------cliente

void cadastrar_cli();
void listar_cli();
void buscar_cli();
void editar_cli();
void excluir_cli();
//--------------------------------------------------------------
void cadastrar_vei();
void listar_vei();
void buscar_vei();
void editar_vei();
void excluir_vei();

//---------------------------------------------------------------
void locacao();
int op7;
int op8;
int diaria;
int dias;
//---------------------------------------------------------------
void carlocar();
int op9;
int op10;
//--------------------------------------------------------------
void devolucao();
void contrato();
void login();
//--------------------------------------------------------------
int posicao;
int achar;
int op1;

void flush_in();
//------------------------------------------------------------
FILE * arq;
FILE * arqv;
FILE * arqj;

//int op;
//------------------------------------------------------------------------------------------
void gotoxy(int x,int y){
    COORD coord;
    coord.X = x;
    coord.Y = y;
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE),coord);

}
void cadastrar_vei(){
    setlocale(LC_ALL,"portuguese");
    voltar:
arqv=fopen("data2.dat","a+b");
v.ativo2=1;//chave de indicação do estado do veículo 1)-não locado, 2)-locado, 0)-foi pra lixeira

setbuf(stdin,NULL);
printf("Digite a marca do veículo: ");
gets(v.marca); //uso de gets permite pegar toda a amplitude das informações com espaços até /n
printf("\nDigite o modelo: ");
gets(v.modelo);//em todo o
setbuf(stdin,NULL);
printf("\nDigite a placa: ");
gets(v.placa);
printf("\nDigite o ano: ");
gets(v.ano);
printf("\nDigite a(s) cor(es):");
gets(v.cor);
printf("\n\n\n");
      printf("Escolha uma opção:\n");
      printf("1)-confirmar cadastro veículo\n");
      printf("2)-alterar cadastro veículo\n");
      printf("0)-cancelar cadastro veículo\n");
      scanf("%d",&op1);

      if(op1==1){
        system("cls");
            system("color a");//cor verde
      printf("Cadastro realizado com sucesso!\n");
      fwrite(&v,sizeof(vei),1,arqv);//escreve as informações preenchidas anteriormente dentro do arquivo

           fclose(arqv);//fecha arquivo veículo
      }
      if(op1==2){
            system("cls");
        goto voltar;//voltar para alterar informações do cadastro
      }
      if(op1==0){
        system("cls");
       // break;
      }


}
void listar_vei(){
setlocale(LC_ALL,"portuguese");
arqv=fopen("data2.dat","rb");
while(fread(&v,sizeof(vei),1,arqv)==1){//fread para ler o fwrite da função cadastrar
    if(v.ativo2!=0){

    if(v.ativo2==2){//lista e informa se está locado

	printf("            _____________________________________________________\n");
	printf("            |                     VEÍCULO                        |\n");
	printf("            |____________________________________________________|\n");
    printf("            |Marca: %s\n",v.marca);
    printf("            |Modelo: %s\n",v.modelo);
    printf("            |Placa %s\n",v.placa);
    printf("            |Ano: %s\n",v.ano);
    printf("            |Cor: %s\n",v.cor);
    printf("            |Situação: locado\n");
	printf("            |____________________________________________________\n\n");

    }
    if(v.ativo2==1){// lista e informa que está disponível para locação
    printf("            _____________________________________________________\n");
	printf("            |                     VEÍCULO                        |\n");
	printf("            |____________________________________________________|\n");
    printf("            |Marca: %s\n",v.marca);
    printf("            |Modelo: %s\n",v.modelo);
    printf("            |Placa %s\n",v.placa);
    printf("            |Ano: %s\n",v.ano);
    printf("            |Cor: %s\n",v.cor);
    printf("            |Situação: não locado\n");
	printf("            |____________________________________________________\n\n");

    }

    }

}
fclose(arqv);


}
void editar_vei(){
setlocale(LC_ALL,"portuguese");
posicao=0;
achar=0;
//int ativo2; char conect[16];char modelo[30],ano[20],placa[20],cor[20];
char placa_[20];
printf("Digite a placa do veículo a ser buscada: ");
gets(placa_);
arqv=fopen("data2.dat","r+b");

system("cls");
op1=-1;
while(fread(&v,sizeof(vei),1,arqv)==1 && op1!=0){
    if(strcmp(placa_,v.placa)==0 && v.ativo2==1){
                         printf("1)-Marca: %s\n",v.marca);
                         printf("2)-Modelo: %s\n",v.modelo);
                         printf("3)-Placa: %s\n",v.placa);
                         printf("4)-Ano: %s\n",v.ano);
                         printf("5)-Cor(es): %s\n",v.cor);
                         printf("0)-Sair\n");

                         printf("\n\nEscolha um dos campos a ser modificado:");
                         scanf("%d",&op1);
                         flush_in();
                         system("cls");

                         switch(op1){

                             //printf("Digite o novo nome:");

                        case 1:printf("Digite a Marca: ");
                         gets(v.marca);
                         fseek(arqv,posicao,SEEK_SET);//posiciona o arquivo na linha desejada
                         achar=fwrite(&v,sizeof(vei),1,arqv)==sizeof(vei);//sobreescreve a linha
                         printf("Campo Marca alterado com sucesso!\n");
                         break;
                     case 2:printf("Digite o Modelo: ");
                         gets(v.modelo);
                         fseek(arqv,posicao,SEEK_SET);
                         achar=fwrite(&v,sizeof(vei),1,arqv)==sizeof(vei);
                         printf("Campo Modelo alterado com sucesso!\n");
                         break;

                     case 3:printf("Digite a Placa: ");
                         gets(v.placa);
                         fseek(arqv,posicao,SEEK_SET);
                         achar=fwrite(&v,sizeof(vei),1,arqv)==sizeof(vei);
                         printf("Campo Placa alterado com sucesso!\n");
                         break;

                     case 4:printf("Digite o Ano: ");
                         gets(v.ano);
                         fseek(arqv,posicao,SEEK_SET);
                         achar=fwrite(&v,sizeof(vei),1,arqv)==sizeof(vei);
                         printf("Campo Placa alterado com sucesso!\n");
                         break;
                     case 5:printf("Digite a(s) cor(es): ");
                         gets(v.cor);
                         fseek(arqv,posicao,SEEK_SET);
                         achar=fwrite(&v,sizeof(vei),1,arqv)==sizeof(vei);
                         printf("Campo cor(es) alterado com sucesso!\n");
                         break;
                        case 0: break;
                         }
                          }

    posicao=posicao+sizeof(vei);
    fseek(arqv,posicao,SEEK_SET);
                          }
                          op1=-1;
                          fclose(arqv);


                           }
void excluir_vei(){
setlocale(LC_ALL,"portuguese");
posicao=0;
achar=0;
int g2;
char placa__[20];
printf("Digite a placa a ser buscada: ");
gets(placa__);
arqv=fopen("data2.dat","r+b");
system("cls");
op1=-1;
while(fread(&v,sizeof(vei),1,arqv)==1 && op1!=0){
    if(strcmp(placa__,v.placa)==0 && v.ativo2==1){


    //printf("Status: %d\n",v.ativo2);
    printf("            _____________________________________________________\n");
	printf("            |                     VEÍCULO                        |\n");
	printf("            |____________________________________________________|\n");
    printf("            |Marca: %s\n",v.marca);
    printf("            |Modelo: %s\n",v.modelo);
    printf("            |Placa %s\n",v.placa);
    printf("            |Ano: %s\n",v.ano);
    printf("            |Cor: %s\n",v.cor);
    printf("            |Situação: não locado\n");
	printf("            |____________________________________________________\n\n");

    printf("          Deseja excluir esse cadastro?\n");
    printf("          1)-sim\n");
    printf("          2)-não\n");
       scanf("%d",&g2);
       if(g2==1){
            system("color a");
        printf("cadastro excluido com sucesso!\n");
      v.ativo2=0;
      fseek(arqv,posicao,SEEK_SET);
      achar=fwrite(&v,sizeof(vei),1,arqv)==sizeof(vei);
       break;
     }
     if(g2==0){
        break;
     }
    }
     posicao=posicao+sizeof(vei);
    fseek(arqv,posicao,SEEK_SET);

}
op1=-1;
fclose(arqv);


}
void buscar_vei(){
setlocale(LC_ALL,"portuguese");
posicao=0;
achar=0;
char placa[20];
printf("Digite a placa a ser buscada: ");
gets(placa);
arqv=fopen("data2.dat","r+b");
system("cls");
op1=-1;
// colocar esse pra busca slide
while(fread(&v,sizeof(vei),1,arqv)==1 && op1!=0){
    if(strcmp(placa,v.placa)==0 ){

        if(v.ativo2==1){

    printf("            _____________________________________________________\n");
	printf("            |                     VEÍCULO                        |\n");
	printf("            |____________________________________________________|\n");
    printf("            |Marca: %s\n",v.marca);
    printf("            |Modelo: %s\n",v.modelo);
    printf("            |Placa %s\n",v.placa);
    printf("            |Ano: %s\n",v.ano);
    printf("            |Cor: %s\n",v.cor);
    printf("            |Situação: não locado\n");
	printf("            |____________________________________________________\n\n");




        }
        if(v.ativo2==2){
    printf("            _____________________________________________________\n");
	printf("            |                     VEÍCULO                        |\n");
	printf("            |____________________________________________________|\n");
    printf("            |Marca: %s\n",v.marca);
    printf("            |Modelo: %s\n",v.modelo);
    printf("            |Placa %s\n",v.placa);
    printf("            |Ano: %s\n",v.ano);
    printf("            |Cor: %s\n",v.cor);
    printf("            |Situação: locado\n");
	printf("            |____________________________________________________\n\n");

    }

    break;
}
 posicao=posicao+sizeof(vei);
    fseek(arqv,posicao,SEEK_SET);
}
op1=-1;
fclose(arqv);
}
void cadastrar_cli(){
                       ir:
                        //  flush_in();
    setlocale(LC_ALL,"portuguese");               //char nome[50],rg[16],cpf[16],nasc[20],rua[100],bairro[100],cidade[20],estado[20],cep[10]; int ativo;

     arq=fopen("data.dat","a+b");
      // flush_in();
    // system("cls");
     c.ativo=1;
  setbuf(stdin,NULL);
    printf("Digite o nome: ");
    gets(c.nome);
      setbuf(stdin,NULL);

    printf("Digite a data de nascimento: ");
    gets(c.nasc);
    printf("Digite o RG: ");
    gets(c.rg);
    printf("Digite o CPF: ");
    gets(c.cpf);
   //endereço
    printf("Digite a rua: ");
    gets(c.rua);
    printf("Digite o bairro: ");
    gets(c.bairro);
    printf("Numero da casa:");
    gets(c.ncasa);
    printf("Digite a cidade: ");
    gets(c.cidade);
    printf("Digite o estado: ");
    gets(c.estado);
    printf("Digite o Cep:");
    gets(c.cep);
    printf("Digite o número de contato:");
    gets(c.telefone);

   // c.ativo=1;
      printf("\n\n\n\n\n");
      printf("Escolha uma opção:\n");
      printf("1)-confirmar cadastro\n");
      printf("2)-alterar cadastro\n");
      printf("0)-cancelar cadastro\n");
      scanf("%d",&op1);
      if(op1==1){
            system("cls");
            system("color a");
      printf("Cadastro realizado com sucesso!\n");
           fwrite(&c,sizeof(cli),1,arq);
           fclose(arq);
      }
      if(op1==2){
            system("cls");
        goto ir;
      }
     if(op1==0){
       //break;
     }
                  }
void listar_cli(){
 arq=fopen("data.dat","rb");
 while(fread(&c,sizeof(cli),1,arq)==1){
        //printf(" |%s |%s |%s |%s |%s |%s |%s |%s |%s|\n",c.nome,c.nasc,c.rg,c.cpf,c.rua,c.bairro,c.cidade,c.estado,c.cep);
       //  printf("Nome\tNascimento\tRg\tCPF\tRua\tBairro\tN° cas\tCidade\tEstado\tCep\tN° contato\n");

if(c.ativo>0){



	printf("           _____________________________________________________\n");
	printf("           |                     CLIENTE                        |\n");
	printf("           |____________________________________________________|\n");
    printf("           |  Nome: %s\n",c.nome);
    printf("           |  Nascimento: %s\n",c.nasc);
    printf("           |  RG: %s\n",c.rg);
    printf("           |  CPF: %s\n",c.cpf);
	printf("           | \n");
    printf("           | LOCALIZAÇÃO============\n");
    printf("           | \n");
    printf("           |  Rua: %s\n",c.rua);
    printf("           |  Bairro: %s\n",c.bairro);
    printf("           |  Nº casa: %s\n",c.ncasa);
    printf("           |  Cidade: %s\n",c.cidade);
    printf("           |  Estado: %s\n",c.estado);
    printf("           |  Cep: %s\n",c.cep);
    printf("           |  Nº contato: %s\n",c.telefone);
    printf("           |_____________________________________________________\n\n");






}

 }
 fclose(arq);
}
 void editar_cli(){
setlocale(LC_ALL,"portuguese");
posicao=0;
achar=0;
char cpf_[16];

//flush_in();
printf("Digite o CPF do cliente a ser encontrado: ");
gets(cpf_);

arq=fopen("data.dat","r+b");


system("cls");
//system("pause");
op1=-1;
 while(fread(&c,sizeof(cli),1,arq)==1 && op1!=0){
    if(strcmp(cpf_,c.cpf)==0){
        if(c.ativo==1){




    printf("   1)-Nome: %s\n",c.nome);
    printf("   2)-Data de Nascimento: %s\n",c.nasc);
    printf("   3)-RG: %s\n",c.rg);
    printf("   4)-CPF: %s\n",c.cpf);
    printf("   5)-Rua: %s\n",c.rua);
    printf("   6)-Bairro: %s\n",c.bairro);
    printf("   7)-Número da casa: %s\n",c.ncasa);
    printf("   8)-Cidade: %s\n",c.cidade);
    printf("   9)-Estado: %s\n",c.estado);
    printf("   10)-Cep: %s\n",c.cep);
    printf("   11)-Telefone: %s\n",c.telefone);
    printf("   0)-sair\n\n ");





   printf("    Escolha uma dos campos a ser  modificado: ");
             scanf("%d",&op1);
             flush_in();
             system("cls");
           // op1=op1;
        switch(op1){
     // case 0:printf("Apenas para clientes que desejam fechar cadastro!\n");
      //break;
    case 1:printf("Digite o novo nome:");
      gets(c.nome);
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
            system("color a");
           system("cls");

      printf("Campo nome alterado com sucesso!\n");
       break;
    case 2:printf("Digite a nova data de nascimento:");
      gets(c.nasc);
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
            system("color a");
           system("cls");

     printf("Campo data de nascimento alterado com sucesso!\n");
       break;
    case 3:printf("Digite o novo RG:");
      gets(c.rg);
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
            system("color a");
           system("cls");

    printf("Campo RG alterado com sucesso!\n");
       break;
    case 4:printf("Digite o novo cpf:");
      gets(c.cpf);
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
            system("color a");
           system("cls");

        printf("Campo CPF alterado com sucesso!\n");

       break;
    case 5:printf("Digite a nova rua:");
      gets(c.rua);
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
            system("color a");
           system("cls");

        printf("Campo rua alterado com sucesso!\n");

       break;
    case 6:printf("Digite o novo bairro:");
      gets(c.bairro);
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
            system("color a");
           system("cls");

        printf("Campo bairro alterado com sucesso!\n");
       break;
       case 7:printf("Digite o novo número da casa:");
      gets(c.ncasa);
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
            system("color a");
           system("cls");

        printf("Campo número da casa alterado com sucesso!\n");
       break;

    case 8:printf("Digite a nova cidade:");
      gets(c.cidade);
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
            system("color a");
           system("cls");

       printf("Campo cidade alterado com sucesso!\n");
       break;
    case 9:printf("Digite o novo estado:");
      gets(c.estado);
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
            system("color a");
           system("cls");

        printf("Campo estado alterado com sucesso!\n");
       break;
    case 10:printf("Digite o novo cep:");
      gets(c.cep);
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
            system("color a");
           system("cls");

        printf("Campo Cep alterado com sucesso!\n");
       break;

       case 11:printf("Digite o novo número de contato(telefone):");
      gets(c.telefone);
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
      system("color a");
                 system("cls");

      printf("Campo número de contato alterado com sucesso!\n");
       break;

    case 0:break;

        }

    }
    }
    posicao=posicao+sizeof(cli);
    fseek(arq,posicao,SEEK_SET);
}

op1=-1;
fclose(arq);



 }
void excluir_cli(){
 posicao=0;
setlocale(LC_ALL,"portuguese");

int g;
char cpf__[16];
//flush_in();
printf("Digite o CPF do cliente a ser encontrado: ");
gets(cpf__);

arq=fopen("data.dat","r+b");


system("cls");
//system("pause");
op1=-1;
 while(fread(&c,sizeof(cli),1,arq)==1 && op1!=0){
    if(strcmp(cpf__,c.cpf)==0 && c.ativo==1){


    printf("           _____________________________________________________\n");
	printf("           |                     CLIENTE                        |\n");
	printf("           |____________________________________________________|\n");
    printf("           |  Nome: %s\n",c.nome);
    printf("           |  Nascimento: %s\n",c.nasc);
    printf("           |  RG: %s\n",c.rg);
    printf("           |  CPF: %s\n",c.cpf);
	printf("           | \n");
    printf("           | LOCALIZAÇÃO============\n");
    printf("           | \n");
    printf("           |  Rua: %s\n",c.rua);
    printf("           |  Bairro: %s\n",c.bairro);
    printf("           |  Nº casa: %s\n",c.ncasa);
    printf("           |  Cidade: %s\n",c.cidade);
    printf("           |  Estado: %s\n",c.estado);
    printf("           |  Cep: %s\n",c.cep);
    printf("           |  Nº contato: %s\n",c.telefone);
    printf("           |_____________________________________________________\n\n");





       printf("Deseja excluir esse cadastro?\n");
       printf("1)-sim\n");
       printf("2)-não\n");
       scanf("%d",&g);

     if(g==1){
            system("color a");
        printf("cadastro excluido com sucesso!\n");
      c.ativo=0;
      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
       break;
     }
     if(g==0){
        break;
     }










    }
    posicao=posicao+sizeof(cli);
    fseek(arq,posicao,SEEK_SET);
}

op1=-1;
fclose(arq);




}
void buscar_cli(){
    posicao=0;
achar=0;
setlocale(LC_ALL,"portuguese");
    char cpff[16];
    printf("Digite o CPF do cliente a ser encontrado: ");
gets(cpff);

arq=fopen("data.dat","r+b");


system("cls");
//system("pause");
op1=-1;
 while(fread(&c,sizeof(cli),1,arq)==1 && op1!=0){
    if(strcmp(cpff,c.cpf)==0 && c.ativo==1){


    printf("           _____________________________________________________\n");
	printf("           |                     CLIENTE                        |\n");
	printf("           |____________________________________________________|\n");
    printf("           |  Nome: %s\n",c.nome);
    printf("           |  Nascimento: %s\n",c.nasc);
    printf("           |  RG: %s\n",c.rg);
    printf("           |  CPF: %s\n",c.cpf);
	printf("           | \n");
    printf("           | LOCALIZAÇÃO============\n");
    printf("           | \n");
    printf("           |  Rua: %s\n",c.rua);
    printf("           |  Bairro: %s\n",c.bairro);
    printf("           |  Nº casa: %s\n",c.ncasa);
    printf("           |  Cidade: %s\n",c.cidade);
    printf("           |  Estado: %s\n",c.estado);
    printf("           |  Cep: %s\n",c.cep);
    printf("           |  Nº contato: %s\n",c.telefone);
    printf("           |_____________________________________________________\n\n");






       break;
          }
    posicao=posicao+sizeof(cli);
    fseek(arq,posicao,SEEK_SET);
}

op1=-1;
fclose(arq);






}
void main(){
    cli c;

setlocale(LC_ALL,"portuguese");
int a,b,t,op;
char ussuario[20]="usu@rio";
//char senha
char us[20],senh[20];
                              gotoxy(30,10);

                        printf("!!!!!!!Bem Vindo ao Sistema de Locação de Veículos !!!!!!!\n\n\n\n");

                        printf("                                             Digite o nome de usuário: ");
                        gets(us);
                        printf("                                                Digite a senha:  ");
                        gets(senh);
                      if(strcmp(us,usuario)==0 && strcmp(senh,senha)==0){



  do{
  op=0;
  a=3;
  b=3;

system("cls");
         system("color 7");
    printf("                      ___________________________________________________________\n");
    printf("                     |_______________________Menu Principal_____________________|\n");
    printf("                     |__________________________________________________________|\n");
	printf("                     | [   ]1- Cliente                                          |\n");
	printf("                     | [   ]2- veículo                                          |\n");
	printf("                     | [   ]3- Locação                                          |\n");
	printf("                     | [   ]4- veículos locados                                 |\n");
	printf("                     | [   ]5- Gerador de Contrato                              | \n");
    printf("                     | [   ]6- Devolução do veículo                             | \n");
    printf("                     | [   ]7- Lixeira                                          | \n");
	printf("                     | [   ]8- EXIT                                             |\n");
    printf("                     +__________________________________________________________+");
    printf("\n\n\n\n");


    //-------------------------------------------------------------------------------------------------------
    do{
    gotoxy(24,a);
    printf("-->");
    gotoxy(1,20);
    t=getch();
    if(t==80 && a<10){
        b=a;
        a++;
    }
    if(t==72 && a>3){
        b=a;
        a--;
    }
    if(b!=a){
        gotoxy(24,b);
        printf("   ");

    }
    if(t==13){

        op=a-2;
    }

    }while(op==0);
//----------------------------------------------------------------------------------------------------------------
    switch(op){

    case 1: system("cls");
         int d,c,h,op2;

        do{
            op2=0;
            d=3;
            c=3;
            system("cls");
            system("color 7");

    printf("                     ____________________________________________________________\n");
    printf("                     |_______________________ MENU CLIENTE _____________________|\n");
    printf("                     |__________________________________________________________|\n");
	printf("                     | [   ]1- Cadastrar Cliente                                |\n");
	printf("                     | [   ]2- listar cliente                                   |\n");
	printf("                     | [   ]3- buscar cliente                                   |\n");
	printf("                     | [   ]4- alterar cadastro                                 |\n");
	printf("                     | [   ]5- excluir cadastro                                 | \n");
	printf("                     | [   ]6- voltar                                           |\n");
    printf("                     +__________________________________________________________+");
    printf("\n\n\n\n");

             do{
                gotoxy(24,c);
                printf("-->");
                gotoxy(1,10);
                h=getch();
                if(h==80 && c<8){
                    d=c;
                    c++;
                }
                if(h==72 && c>3){
                    d=c;
                    c--;
                }
                if(d!=c){
                    gotoxy(24,d);
                    printf("   ");

                }
                if(h==13){
                    op2=c-2;
                    }

             }while(op2==0);
             //-------------------------------------------------------------------------------------------------------------------
                   switch(op2){

                   case 1: system("cls");
                   cadastrar_cli();
                   system("pause");
                   break;
                   case 2:
                   system("cls");
                   listar_cli();
                   system("pause");
                   break;
                   case 3:
                   system("cls");
                     buscar_cli();
                   system("pause");
                   break;
                   case 4:system("cls");
                   // printf("                                             Digite o nome de usuário: ");
                    //   gets(us);
                     //   printf("                                                Digite a senha:  ");
                      //  gets(senh);
                     // if(strcmp(us,usuario)==0 && strcmp(senh,senha)==0){
                    //  system("cls");
                    editar_cli();
                     // }
                 system("pause");
                 break;
                 case 5:system("cls");
                    // printf("                                             Digite o nome de usuário: ");
                      //  gets(us);
                      //  printf("                                                Digite a senha:  ");
                      //  gets(senh);
                     // if(strcmp(us,usuario)==0 && strcmp(senh,senha)==0){
                     // system("cls");
                  excluir_cli();
                    //  }
                 system("pause");

             }


        }while(op2!=6);
        break;









//-----------------------------------------------------------------------------------------

              case 2: system("cls");

         int o,p,tt,op12;

        do{
            op12=0;
            o=3;
            p=3;
            system("cls");
            system("color 7");


    printf("                     ____________________________________________________________\n");
    printf("                     |________________________ MENU VEÍCULO_____________________|\n");
    printf("                     |__________________________________________________________|\n");
	printf("                     | [   ]1- Cadastrar veículo                                |\n");
	printf("                     | [   ]2- Listar veículos Cadastrados                      |\n");
	printf("                     | [   ]3- Buscar veículo                                   |\n");
	printf("                     | [   ]4- Alterar Cadastro veículo                         |\n");
	printf("                     | [   ]5- Excluir Cadastro veículo                         | \n");
	printf("                     | [   ]6- Voltar                                           |\n");
    printf("                     +__________________________________________________________+");
    printf("\n\n\n\n");



             do{
                gotoxy(24,o);
                printf("-->");
                gotoxy(1,10);
                tt=getch();
                if(tt==80 && o<8){
                    p=o;
                    o++;
                }
                if(tt==72 && o>3){
                    p=o;
                    o--;
                }
                if(p!=o){
                    gotoxy(24,p);
                    printf("   ");

                }
                if(tt==13){
                    op12=o-2;
                    }

             }while(op12==0);
             //-------------------------------------------------------------------------------------------------------------------
                   switch(op12){

                   case 1: system("cls");
                   cadastrar_vei();
                   system("pause");
                   break;
                   case 2:
                   system("cls");
                   listar_vei();
                   system("pause");
                   break;
                   case 3:
                   system("cls");
                     buscar_vei();
                   system("pause");
                   break;
                   case 4:system("cls");
                       setbuf(stdin,NULL);
                   // printf("                                             Digite o nome de usuário: ");
                      //  gets(us);
                      //  printf("                                                Digite a senha:  ");
                       // gets(senh);
                     // if(strcmp(us,usuario)==0 && strcmp(senh,senha)==0){
                         //  system("cls");
                    editar_vei();
                   //   }
                 system("pause");
                 break;
                 case 5:system("cls");
                    // setbuf(stdin,NULL);
                 // printf("                                             Digite o nome de usuário: ");
                    //    gets(us);
                    //    printf("                                                Digite a senha:  ");
                      //  gets(senh);
                     // if(strcmp(us,usuario)==0 && strcmp(senh,senha)==0){
                    //  system("cls");
                 excluir_vei();
                  //    }
               system("pause");
                 break;
             }


        }while(op12!=6);
        break;
//------------------------------------------------------------------------------------------------

                 case 3:system("cls");
                 locacao();

                 system("pause");
                 break;
                 case 4: system("cls");
                 carlocar();
                 system("pause");
                 break;
                 case 5:system("cls");
                 contrato();
                 system("pause");
                 break;
                 case 6: system("cls");
                devolucao();
                 system("pause");
                 break;

                  case 7: system("cls");
                                setbuf(stdin,NULL);
                                /*
                        printf("                                             Digite o nome de usuário: ");
                        gets(us);
                        printf("                                                Digite a senha:  ");
                        gets(senh);
                      if(strcmp(us,usuario)==0 && strcmp(senh,senha)==0){
                         */
         //int o,p,tt,op12;
         int o2,p2,tt2,op13;

        do{
            op13=0;
            o2=3;
            p2=3;
            system("cls");
            system("color 7");




    printf("                      ___________________________________________________________\n");
    printf("                     |_______________________Menu LIXEIRA_______________________|\n");
    printf("                     |__________________________________________________________|\n");
	printf("                     | [   ]1- Listar Cadastros/cliente                         |\n");
	printf("                     | [   ]2- Excluir cadastro/cliente definitivamente         |\n");
    printf("                     | [   ]3- Restaurar cadastro/cliente                       |\n");
    printf("                     | [   ]4- Listar Cadastro/Veículos                         |\n");
    printf("                     | [   ]5- Excluir Cadastro/Veículo                         |\n");
	printf("                     | [   ]6- Restaurar cadastro/veículo                       |\n");
	printf("                     | [   ]7- voltar                                           |\n");
	printf("                     |                                                          |\n");
    printf("                     +__________________________________________________________+");
    printf("\n\n\n\n");

             do{
                gotoxy(24,o2);
                printf("-->");
                gotoxy(1,10);
                tt2=getch();
                if(tt2==80 && o2<9){
                    p2=o2;
                    o2++;
                }
                if(tt2==72 && o2>3){
                    p2=o2;
                    o2--;
                }
                if(p2!=o2){
                    gotoxy(24,p2);
                    printf("   ");

                }
                if(tt2==13){
                    op13=o2-2;
                    }

             }while(op13==0);
             //-------------------------------------------------------------------------------------------------------------------
                   switch(op13){

                   case 1: system("cls");
                    listar_cli_lix();
                   system("pause");
                   break;
                   case 2:
                   system("cls");
                    excluir_cli_lix();
                   system("pause");
                   break;
                   case 3:
                       system("cls");
                      rest_cli_lix();
                       system("pause");
                       break;
                   case 4:
                    system("cls");
                  listar_vei_lix();
                       system("pause");
                       break;
                       case 5: system("cls");
                    excluir_vei_lix();
                       system("pause");
                       case 6:system("cls");
                        rest_vei_lix();
                       system("pause");
                        case 7:break;

             }


        }while(op13!=7);
                     // }
        break;
//------------------------------------------------------------------------------------------------

                  system("pause");

                           break;
                        case 8: system("cls");
                        system("pause");
                        break;



                      }

}while(op!=8);

}
}





void flush_in(){
int ch;
while( (ch=fgetc(stdin) )!='\n'&&ch!=EOF){}
 }
void locacao(){
setlocale(LC_ALL,"portuguese");
int psc=0;
int acc=0;
int psv=0;
int acv=0;
int g3;
char placav[20];
char cpfc[16];
setbuf(stdin,NULL);
printf("Digite a placa a ser buscada: ");
gets(placav);
setbuf(stdin,NULL);
printf("Digite o cpf do cliente a ser buscado: ");
gets(cpfc);

arq=fopen("data.dat","r+b");
arqv=fopen("data2.dat","r+b");
arqj=fopen("data3.dat","a+b");
system("cls");
op7=-1;
op8=-1;
while(fread(&c,sizeof(cli),1,arq)==1 && op7!=0){
        if( strcmp(cpfc,c.cpf)==0){
while(fread(&v,sizeof(vei),1,arqv)==1 && op8!=0){
    if(strcmp(placav,v.placa)==0 && v.ativo2==1){



    printf("           _____________________________________________________\n");
	printf("           |                     CLIENTE                        |\n");
	printf("           |____________________________________________________|\n");
    printf("           |  Nome: %s\n",c.nome);
    printf("           |  Nascimento: %s\n",c.nasc);
    printf("           |  RG: %s\n",c.rg);
    printf("           |  CPF: %s\n",c.cpf);
	printf("           | \n");
    printf("           | LOCALIZAÇÃO============\n");
    printf("           | \n");
    printf("           |  Rua: %s\n",c.rua);
    printf("           |  Bairro: %s\n",c.bairro);
    printf("           |  Nº casa: %s\n",c.ncasa);
    printf("           |  Cidade: %s\n",c.cidade);
    printf("           |  Estado: %s\n",c.estado);
    printf("           |  Cep: %s\n",c.cep);
    printf("           |  Nº contato: %s\n",c.telefone);
    printf("           |_____________________________________________________\n\n");



    printf("            _____________________________________________________\n");
	printf("            |                     VEÍCULO                        |\n");
	printf("            |____________________________________________________|\n");
    printf("            |Marca: %s\n",v.marca);
    printf("            |Modelo: %s\n",v.modelo);
    printf("            |Placa %s\n",v.placa);
    printf("            |Ano: %s\n",v.ano);
    printf("            |Cor: %s\n",v.cor);
    printf("            |Situação: não locado\n");
	printf("            |____________________________________________________\n\n");

    printf("            Deseja locar o veículo?\n");
    printf("            1)-sim\n");
    printf("            2)-não\n");
     scanf("%d",&g3);

     if(g3==1){
                j.ativo3=1;
               // char datatual[20],datdev[20];
//int preco,qtd,ptotal;
setbuf(stdin,NULL);
printf("\nDigite a data atual da locação:");
gets(j.datatual);
setbuf(stdin,NULL);
printf("\nDigite a data de devolucação:");
gets(j.datdev);


setbuf(stdin,NULL);
printf("\nDigite o preço da diária: ");
gets(j.preco);
setbuf(stdin,NULL);
printf("\nDigite a quantidade de dias até a devolução: ");
gets(j.qtd);
setbuf(stdin,NULL);
printf("\nDigite o preço total a ser pago: ");
gets(j.ptotal);


                strcpy(j.marca,v.marca);
             strcpy(j.modelo,v.modelo);
            strcpy(j.placa,v.placa);
            strcpy(j.ano,v.ano);
            strcpy(j.cor,v.cor);
            //-------------------------------
            strcpy(j.nome,c.nome);
            strcpy(j.nasc,c.nasc);
            strcpy(j.rg,c.rg);
            strcpy(j.cpf,c.cpf);
            strcpy(j.rua,c.rua);
            strcpy(j.ncasa,c.ncasa);

            strcpy(j.bairro,c.bairro);
            strcpy(j.cidade,c.cidade);
            strcpy(j.estado,c.estado);
            strcpy(j.cep,c.cep);
            strcpy(j.telefone,c.telefone);


             fwrite(&j,sizeof(jun),1,arqj);
                fclose(arqj);


                //------------------------------------------------------

        v.ativo2=2;

        system("color a");
        system("cls");
        printf("Veículo locado com sucesso: ");
         fseek(arqv,psv,SEEK_SET);
      acv=fwrite(&v,sizeof(vei),1,arqv)==sizeof(vei);
     }
        if(g3==2){
            break;
        }
    }
     psv=psv+sizeof(vei);
    fseek(arqv,psv,SEEK_SET);
}
op8=-1;
fclose(arqv);
        }
psc=psc+sizeof(cli);
   fseek(arq,psc,SEEK_SET);
}
op7=-1;
fclose(arq);
}
void carlocar(){
   //lista os veículos locados e os respectivos clientes que estão fazendo o uso.
  arqj=fopen("data3.dat","rb");
while(fread(&j,sizeof(jun),1,arqj)==1){
    if(j.ativo3){

/*
    printf("=========veículo=====\n");
    printf("Marca: %s\n",j.marca);
    printf("Modelo: %s\n",j.modelo);
    printf("Placa %s\n",j.placa);
    printf("Ano: %s\n",j.ano);
    printf("Cor: %s\n",j.cor);
    printf("=========================\n\n");



   printf("========Cliente=========\n");

    printf("Nome: %s\n",j.nome);
    printf("Data de Nascimento: %s\n",j.nasc);
    printf("RG: %s\n",j.rg);
    printf("CPF: %s\n",j.cpf);
    printf("=========Localização=====");
    printf("\nRua: %s\n",j.rua);
    printf("Bairro: %s\n",j.bairro);
    printf("Número da casa: %s\n",j.ncasa);
    printf("Cidade: %s\n",j.cidade);
    printf("Estado: %s\n",j.estado);
    printf("Cep: %s\n",j.cep);
    printf("Número da contato: %s\n",j.telefone);
    printf("=========================\n\n\n");
*/
    printf("           _____________________________________________________\n");
	printf("           |                     CLIENTE                        |\n");
	printf("           |____________________________________________________|\n");
    printf("           |  Nome: %s\n",j.nome);
    printf("           |  Nascimento: %s\n",j.nasc);
    printf("           |  RG: %s\n",j.rg);
    printf("           |  CPF: %s\n",j.cpf);
	printf("           | \n");
    printf("           | LOCALIZAÇÃO============\n");
    printf("           | \n");
    printf("           |  Rua: %s\n",j.rua);
    printf("           |  Bairro: %s\n",j.bairro);
    printf("           |  Nº casa: %s\n",j.ncasa);
    printf("           |  Cidade: %s\n",j.cidade);
    printf("           |  Estado: %s\n",j.estado);
    printf("           |  Cep: %s\n",j.cep);
    printf("           |  Nº contato: %s\n",c.telefone);
    printf("           |_____________________________________________________\n\n");



    printf("           _____________________________________________________\n");
	printf("           |                     VEÍCULO                        |\n");
	printf("           |____________________________________________________|\n");
    printf("           |Marca: %s\n",j.marca);
    printf("           |Modelo: %s\n",j.modelo);
    printf("           |Placa %s\n",j.placa);
    printf("           |Ano: %s\n",j.ano);
    printf("           |Cor: %s\n",j.cor);
    printf("           |Situação: não locado\n");
	printf("           |____________________________________________________\n\n");





// printf("%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\t%s\n\n",j.marca,j.modelo,j.placa,j.ano,j.cor,j.nome,j.nasc,j.rg,j.cpf,j.rua,j.bairro,j.ncasa,j.cidade,j.estado,j.cep,j.telefone);


   }

}
fclose(arqj);
}
 void devolucao(){
setlocale(LC_ALL,"portuguese");
int psj=0;
int acj=0;
int psv=0;
int acv=0;
int g3;
char placav[20];
setbuf(stdin,NULL);
printf("Digite a placa do veículo a ser devolvido: ");
gets(placav);
char nu[10]=("   ");

arqv=fopen("data2.dat","r+b");
arqj=fopen("data3.dat","r+b");
system("cls");
op7=-1;
op8=-1;
while(fread(&j,sizeof(jun),1,arqj)==1 && op7!=0){
        if( strcmp(placav,j.placa)==0){
while(fread(&v,sizeof(vei),1,arqv)==1 && op8!=0){

    printf("           _____________________________________________________\n");
	printf("           |                     CLIENTE                        |\n");
	printf("           |____________________________________________________|\n");
    printf("           |  Nome: %s\n",j.nome);
    printf("           |  Nascimento: %s\n",j.nasc);
    printf("           |  RG: %s\n",j.rg);
    printf("           |  CPF: %s\n",j.cpf);
	printf("           | \n");
    printf("           | LOCALIZAÇÃO============\n");
    printf("           | \n");
    printf("           |  Rua: %s\n",j.rua);
    printf("           |  Bairro: %s\n",j.bairro);
    printf("           |  Nº casa: %s\n",j.ncasa);
    printf("           |  Cidade: %s\n",j.cidade);
    printf("           |  Estado: %s\n",j.estado);
    printf("           |  Cep: %s\n",j.cep);
    printf("           |  Nº contato: %s\n",c.telefone);
    printf("           |_____________________________________________________\n\n");



    printf("           _____________________________________________________\n");
	printf("           |                     VEÍCULO                        |\n");
	printf("           |____________________________________________________|\n");
    printf("           |Marca: %s\n",j.marca);
    printf("           |Modelo: %s\n",j.modelo);
    printf("           |Placa %s\n",j.placa);
    printf("           |Ano: %s\n",j.ano);
    printf("           |Cor: %s\n",j.cor);
    printf("           |Situação: não locado\n");
	printf("           |____________________________________________________\n\n");





    printf("           Deseja realizar a devolução desse veículo?\n");
    printf("           1)-sim\n");
    printf("           2)-não\n");
     scanf("%d",&g3);

     if(g3==1){
                j.ativo3=0;
             strcpy(j.marca,nu);
             strcpy(j.modelo,nu);
            strcpy(j.placa,nu);
            strcpy(j.ano,nu);
            strcpy(j.cor,nu);
            //-------------------------------
            strcpy(j.nome,nu);
            strcpy(j.nasc,nu);
            strcpy(j.rg,nu);
            strcpy(j.cpf,nu);
            strcpy(j.rua,nu);
            strcpy(j.bairro,nu);
            strcpy(j.ncasa,nu);

            strcpy(j.cidade,nu);
            strcpy(j.estado,nu);
            strcpy(j.cep,nu);
            strcpy(j.telefone,nu);



    //------------------------------------------------------

        v.ativo2=1;

        system("color a");
        printf("Devolução realizada com sucesso: ");

         fseek(arqv,psv,SEEK_SET);
      acv=fwrite(&v,sizeof(vei),1,arqv)==sizeof(vei);

       fseek(arqj,psj,SEEK_SET);
      acj=fwrite(&j,sizeof(jun),1,arqj)==sizeof(jun);
      break;
     }
        if(g3==2){
            break;
        }
    }
     psv=psv+sizeof(vei);
    fseek(arqv,psv,SEEK_SET);



    op8=-1;
fclose(arqv);
}
    psj=psj+sizeof(jun);
    fseek(arqj,psj,SEEK_SET);
}
op7=-1;
fclose(arq);
}

void contrato(){
posicao=0;
        char cpf[16];
setlocale(LC_ALL,"portuguese");
        printf("Digite o cpf a ser buscado: ");
        gets(cpf);
        system("cls");
        op1=-1;
        arqj=fopen("data3.dat","r+b");

        while(fread(&j,sizeof(jun),1,arqj)==1 && op1!=0){
            if(strcmp(cpf,j.cpf)==0){
system("color f0");
       // printf("        _____________________________________________________________________________________________________________________________________________________________________________________________________\n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                      CONTRATO DE LOCAÇÃO DE AUTOMÓVEL DE PRAZO DETERMINADO                                                                                                                   \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                            IDENTIFICAÇÃO DAS PARTES CONTRATANTES                                                                                                                             \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                    LOCADORA:  Quatro Rodas, com sede em (Tianguá), na Rua (Presidente José de Farias), nº (122),                                                                                             \n");
        printf("                    bairro (Marques Dutra), Cep (62320-000), no Estado (Ceará), inscrita no C.N.P.J. sob o nº (87.231.919/0001-74), e no                                                                      \n");
        printf("                    Cadastro Estadual sob o nº (xxx), neste ato representada pelo seu diretor (Dênis Marques Filho Sousa),                                                                                    \n");
        printf("                    (Brasileiro), (Casado), (Gerente de Locadora de veicúlos), Carteira de Identidade nº (16.545.139-7), C.P.F. nº (170.996.003-54),                                                          \n");
        printf("                    residente e domiciliado na Rua (Capitão Roberto Lima), nº (2038), bairro (Ipiranga Sul), Cep (62320-000), Cidade (Tianguá), no Estado (Ceará);                                            \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                    LOCATÁRIO: (%s), Carteira de Identidade nº (%s),C.P.F nº(%s)                                                                                                                              \n",j.nome,j.rg,j.cpf);
        printf("                    Data de Nascimento nº (%s), residente e domiciliado na Rua (%s),                                                                                                                          \n",j.nasc,j.rua);
        printf("                    bairro (%s),nº (%s), Cep (%s), Cidade (%s), no Estado (%s).                                                                                                                              \n",j.bairro,j.ncasa,j.cep,j.cidade,j.estado);
        printf("                                                                                                                                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                    As partes acima identificadas têm, entre si, justo e acertado o presente                                                                                                                  \n");
        printf("                    Contrato de Locação de Automóvel de Prazo Determinado, que se regerá pelas                                                                                                                \n");
        printf("                    cláusulas seguintes e pelas condições descritas no presente.                                                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                              DO OBJETO DO CONTRATO                                                                                                                                           \n");
        printf("                    Cláusula 1ª. O presente contrato tem como OBJETO a locação1 do automóvel                                                                                                                  \n");
        printf("                    marca (%s), modelo (%s), ano (%s), cor (%s), placa (%s)                                                                                                                                   \n",j.marca,j.modelo,j.ano,j.cor,j.placa);
        printf("                                                                                                                                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                   DO USO                                                                                                                                                     \n");
        printf("                    Cláusula 2ª. O automóvel, objeto deste contrato, será utilizado exclusivamente                                                                                                            \n");
        printf("                    pelo LOCATÁRIO, não sendo permitido o seu uso por terceiros sob pena de rescisão                                                                                                          \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                   DA DEVOLUÇÃO                                                                                                                                               \n");
        printf("                    Cláusula 3ª. O LOCATÁRIO deverá devolver o automóvel à LOCADORA nas                                                                                                                       \n");
        printf("                    mesmas condições em que estava quando o recebeu, ou seja, em perfeitas condições de                                                                                                       \n");
        printf("                    uso, respondendo pelos danos ou prejuízos causados2.                                                                                                                                      \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                     DO PRAZO                                                                                                                                                 \n");
        printf("                    Cláusula 4ª. A presente locação com diária no valor de (%s), terá o lapso temporal de validade de (%s) dias,                                                                               \n",j.preco,j.qtd);
        printf("                    iniciando no dia (%s) e terminando no dia (%s), data na qual o automóvel deverá ser devolvido, Contabilizando um total de (%s).                                                             \n",j.datatual,j.datdev,j.ptotal);
        printf("                    Cláusula 5ª. Se o LOCATÁRIO não restituir o automóvel na data estipulada,                                                                                                                 \n");
        printf("                    deverá pagar, enquanto detiver em seu poder, o aluguel que a LOCADORA arbitrar no valor de(%s)R$, e                                                                                       \n",j.preco);
        printf("                    responderá pelo dano, que o automóvel venha a sofrer mesmo se proveniente de caso fortuito3.                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                      DA RESCISÃO                                                                                                                                             \n");
        printf("                    Cláusula 6ª. É assegurado às partes a rescisão do presente contrato a qualquer momento,                                                                                                   \n");
        printf("                    desde que haja comunicação à outra parte com antecedência mínima de (1) dia.                                                                                                              \n");
        printf("                    Cláusula 7ª. O descumprimento de qualquer das cláusulas por parte dos contratantes ensejará a rescisão                                                                                    \n");
        printf("                    deste instrumento e o devido pagamento de multa, pela parte inadimplente no valor de R$ (120) (Cento e vinte reais).                                                                       \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                                                                                                                                                                                                              \n");
        printf("                assinatura do contratado: ___________________________________________________________________________________________________                                                                 \n" );
        printf("                                                                                                                                                                                                              \n");
        printf("                assinatura do contratante: ___________________________________________________________________________________________________                                                                 \n" );
        printf("                                                                                                                                                                                                               \n");
        printf("                                                                                                                                                                                                               \n");


      //  printf("       _______________________________________________________________________________________________________________________________________________________________________________________________________ \n");







}

 posicao=posicao+sizeof(jun);
   fseek(arqj,posicao,SEEK_SET);
            }


            op1=-1;
fclose(arqj);

        }

void listar_cli_lix(){
setlocale(LC_ALL,"portuguese");
arq=fopen("data.dat","rb");
 while(fread(&c,sizeof(cli),1,arq)==1){
        //printf(" |%s |%s |%s |%s |%s |%s |%s |%s |%s|\n",c.nome,c.nasc,c.rg,c.cpf,c.rua,c.bairro,c.cidade,c.estado,c.cep);
if(c.ativo==0){
    printf("           _____________________________________________________\n");
	printf("           |                     CLIENTE                        |\n");
	printf("           |____________________________________________________|\n");
    printf("           |  Nome: %s\n",c.nome);
    printf("           |  Nascimento: %s\n",c.nasc);
    printf("           |  RG: %s\n",c.rg);
    printf("           |  CPF: %s\n",c.cpf);
	printf("           | \n");
    printf("           | LOCALIZAÇÃO============\n");
    printf("           | \n");
    printf("           |  Rua: %s\n",c.rua);
    printf("           |  Bairro: %s\n",c.bairro);
    printf("           |  Nº casa: %s\n",c.ncasa);
    printf("           |  Cidade: %s\n",c.cidade);
    printf("           |  Estado: %s\n",c.estado);
    printf("           |  Cep: %s\n",c.cep);
    printf("           |  Nº contato: %s\n",c.telefone);
    printf("           |_____________________________________________________\n\n");

}

 }
 fclose(arq);




}


void excluir_cli_lix(){
setlocale(LC_ALL,"portuguese");
posicao=0;
achar=0;
char cpf_[16];
char nulo[1]={" "};

//flush_in();
printf("Digite o CPF do cliente a ser encontrado: ");
gets(cpf_);

arq=fopen("data.dat","r+b");


system("cls");
//system("pause");
op1=-1;
 while(fread(&c,sizeof(cli),1,arq)==1 && op1!=0){
    if(strcmp(cpf_,c.cpf)==0 && c.ativo==0){




    printf("           _____________________________________________________\n");
	printf("           |                     CLIENTE                        |\n");
	printf("           |____________________________________________________|\n");
    printf("           |  Nome: %s\n",c.nome);
    printf("           |  Nascimento: %s\n",c.nasc);
    printf("           |  RG: %s\n",c.rg);
    printf("           |  CPF: %s\n",c.cpf);
	printf("           | \n");
    printf("           | LOCALIZAÇÃO============\n");
    printf("           | \n");
    printf("           |  Rua: %s\n",c.rua);
    printf("           |  Bairro: %s\n",c.bairro);
    printf("           |  Nº casa: %s\n",c.ncasa);
    printf("           |  Cidade: %s\n",c.cidade);
    printf("           |  Estado: %s\n",c.estado);
    printf("           |  Cep: %s\n",c.cep);
    printf("           |  Nº contato: %s\n",c.telefone);
    printf("           |_____________________________________________________\n\n");



             printf("Deseja excluir permanentemente esse arquivo?");
                    printf("\n1)-sim ");
                     printf("\n2)-não ");



            scanf("%d",&op1);
             flush_in();
           system("cls");

        switch(op1){

    case 1:

            c.ativo=-1;

            strcpy(c.nome,nulo);
            strcpy(c.nasc,nulo);
            strcpy(c.rg,nulo);
            strcpy(c.cpf,nulo);
            //-------------------------------
            strcpy(c.rua,nulo);
            strcpy(c.bairro,nulo);
            strcpy(c.ncasa,nulo);
            strcpy(c.cidade,nulo);
            strcpy(c.estado,nulo);
            strcpy(c.cep,nulo);
            strcpy(c.telefone,nulo);



      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
       break;

    case 2:break;

        }

    }
    posicao=posicao+sizeof(cli);
    fseek(arq,posicao,SEEK_SET);
}

op1=-1;
fclose(arq);


}

void rest_cli_lix(){

setlocale(LC_ALL,"portuguese");
posicao=0;
achar=0;
char cpf_[16];
char nulo[1]={" "};

//flush_in();
printf("Digite o CPF do cliente a ser encontrado: ");
gets(cpf_);

arq=fopen("data.dat","r+b");


system("cls");
//system("pause");
op1=-1;
 while(fread(&c,sizeof(cli),1,arq)==1 && op1!=0){
    if(strcmp(cpf_,c.cpf)==0 && c.ativo==0){



    printf("           _____________________________________________________\n");
	printf("           |                     CLIENTE                        |\n");
	printf("           |____________________________________________________|\n");
    printf("           |  Nome: %s\n",c.nome);
    printf("           |  Nascimento: %s\n",c.nasc);
    printf("           |  RG: %s\n",c.rg);
    printf("           |  CPF: %s\n",c.cpf);
	printf("           | \n");
    printf("           | LOCALIZAÇÃO============\n");
    printf("           | \n");
    printf("           |  Rua: %s\n",c.rua);
    printf("           |  Bairro: %s\n",c.bairro);
    printf("           |  Nº casa: %s\n",c.ncasa);
    printf("           |  Cidade: %s\n",c.cidade);
    printf("           |  Estado: %s\n",c.estado);
    printf("           |  Cep: %s\n",c.cep);
    printf("           |  Nº contato: %s\n",c.telefone);
    printf("           |_____________________________________________________\n\n");


    printf("          Deseja restaurar  esse arquivo?");
    printf("\n        1)-sim ");
    printf("\n        2)-não ");



            scanf("%d",&op1);
             flush_in();
           system("cls");

        switch(op1){

    case 1:

            c.ativo=1;





      fseek(arq,posicao,SEEK_SET);
      achar=fwrite(&c,sizeof(cli),1,arq)==sizeof(cli);
       break;

    case 2:break;

        }

    }
    posicao=posicao+sizeof(cli);
    fseek(arq,posicao,SEEK_SET);
}

op1=-1;
fclose(arq);



}




void listar_vei_lix(){

    setlocale(LC_ALL,"portuguese");
arqv=fopen("data2.dat","rb");
while(fread(&v,sizeof(vei),1,arqv)==1){
    if(v.ativo2==0){

    printf("            _____________________________________________________\n");
	printf("            |                     VEÍCULO                        |\n");
	printf("            |____________________________________________________|\n");
    printf("            |Marca: %s\n",v.marca);
    printf("            |Modelo: %s\n",v.modelo);
    printf("            |Placa %s\n",v.placa);
    printf("            |Ano: %s\n",v.ano);
    printf("            |Cor: %s\n",v.cor);
	printf("            |____________________________________________________\n\n");

    }



}
fclose(arqv);



}

void excluir_vei_lix(){
setlocale(LC_ALL,"portuguese");
posicao=0;
achar=0;
char nulo[1]={" "};
//int ativo2; char conect[16];char modelo[30],ano[20],placa[20],cor[20];
char placa_[20];
printf("Digite a placa do veículo a ser buscada: ");
gets(placa_);
arqv=fopen("data2.dat","r+b");

system("cls");
op1=-1;
while(fread(&v,sizeof(vei),1,arqv)==1 && op1!=0){
    if(strcmp(placa_,v.placa)==0 && v.ativo2==0){

    printf("            _____________________________________________________\n");
	printf("            |                     VEÍCULO                        |\n");
	printf("            |____________________________________________________|\n");
    printf("            |Marca: %s\n",v.marca);
    printf("            |Modelo: %s\n",v.modelo);
    printf("            |Placa %s\n",v.placa);
    printf("            |Ano: %s\n",v.ano);
    printf("            |Cor: %s\n",v.cor);
	printf("            |____________________________________________________\n\n");


     printf("\n         Deseja excluir permanentemente esse cadastro/veículo:");
     printf("\n         1)-sim");
     printf("\n         2)-não\n");
                         scanf("%d",&op1);
                         flush_in();
                         system("cls");

                         switch(op1){

                        case 1: v.ativo2=-1;
                         strcpy(v.marca,nulo);
                           strcpy(v.modelo,nulo);
                            strcpy(v.placa,nulo);
                             strcpy(v.ano,nulo);
                           strcpy(v.cor,nulo);


                         fseek(arqv,posicao,SEEK_SET);
                         achar=fwrite(&v,sizeof(vei),1,arqv)==sizeof(vei);
                         break;

                     case 2:
                         break;

                         }
                          }

    posicao=posicao+sizeof(vei);
    fseek(arqv,posicao,SEEK_SET);
                          }
                          op1=-1;
                          fclose(arqv);
}

void rest_vei_lix(){

setlocale(LC_ALL,"portuguese");
posicao=0;
achar=0;

//int ativo2; char conect[16];char modelo[30],ano[20],placa[20],cor[20];
char placa_[20];
printf("Digite a placa do veículo a ser buscada: ");
gets(placa_);
arqv=fopen("data2.dat","r+b");

system("cls");
op1=-1;
while(fread(&v,sizeof(vei),1,arqv)==1 && op1!=0){
    if(strcmp(placa_,v.placa)==0 && v.ativo2==0){

    printf("            _____________________________________________________\n");
	printf("            |                     VEÍCULO                        |\n");
	printf("            |____________________________________________________|\n");
    printf("            |Marca: %s\n",v.marca);
    printf("            |Modelo: %s\n",v.modelo);
    printf("            |Placa %s\n",v.placa);
    printf("            |Ano: %s\n",v.ano);
    printf("            |Cor: %s\n",v.cor);
	printf("            |____________________________________________________\n\n");


    printf("\n          Deseja restaurar esse cadastro/veículo:");
    printf("\n          1)-sim");
    printf("\n          2)-não\n");
                         scanf("%d",&op1);
                         flush_in();
                         system("cls");

                         switch(op1){

                        case 1: v.ativo2=1;


                         fseek(arqv,posicao,SEEK_SET);
                         achar=fwrite(&v,sizeof(vei),1,arqv)==sizeof(vei);
                         break;

                     case 2:
                         break;

                         }
                          }

    posicao=posicao+sizeof(vei);
    fseek(arqv,posicao,SEEK_SET);
                          }
                          op1=-1;
                          fclose(arqv);


}



