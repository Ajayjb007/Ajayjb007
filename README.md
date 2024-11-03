
#include<stdio.h>
#include<string.h>
void main(){
    FILE *f1,*f2,*f3,*f4;
    int cur_loc,start,operand,opcode,len;
    char instruction[20],label[20],op[20],symbol[20];
    f1=fopen("input.txt","r");
    f3=fopen("symtab.txt","w+");
    f4=fopen("output.txt","w");
    printf("\n\n### Pass 1 Assembler Program ###\n");
    fscanf(f1,"%s %s %d",label,instruction,&operand);
    if(strcmp(instruction,"START") == 0){
        start = operand;
        cur_loc = start;
        fprintf(f4,"\t%s\t%s\t%d\n",label,instruction,operand);
    }
    else
        cur_loc = 0;
        fscanf(f1,"%s %s",label,instruction);
    while(!feof(f1)){
        fscanf(f1,"%s",op);
        fprintf(f4,"\n%d\t%s\t%s\t%s\n",cur_loc,label,instruction,op);
        if(strcmp(label, "-") != 0){
            fprintf(f3,"\n%d\t%s\n", cur_loc, label);
        }
    f2=fopen("optab.txt","r");
    fscanf(f2,"%s %d",symbol, &opcode);
    while(!feof(f2)){
        if(strcmp(instruction,symbol) == 0){
            cur_loc = cur_loc + 3;
            break;
        }
        fscanf(f2,"%s %d",symbol, &opcode);
    }
    fclose(f2);
    if(strcmp(instruction,"WORD")==0){
        cur_loc=cur_loc+3;
    }
    else if(strcmp(instruction,"RESW")==0){
        operand=atoi(op);
        cur_loc=cur_loc+(3*operand);
    }
    else if(strcmp(instruction,"BYTE")==0){
        if(op[0]=='X')
            cur_loc=cur_loc+1;
        else{
            len=strlen(op)-2;
            cur_loc = cur_loc+len;
        }
    }
    else if(strcmp(instruction,"RESB")==0){
        operand=atoi(op);
        cur_loc=cur_loc+operand;
    }
        fscanf(f1,"%s%s",label,instruction);
    }
    if(strcmp(instruction,"END")==0){
        printf("\n\n Length of the Program = %d\n",cur_loc - start);
    }
    fclose(f1);  fclose(f3);  fclose(f4);
}
