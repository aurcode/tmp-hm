# 1. 稀疏矩阵的乘法运算

```
#include <bits/stdc++.h>
using namespace std;

const int N = 10010;

typedef struct {
    int row, col, data;
} elem;

typedef struct {
    elem *element = (elem*) malloc(N * sizeof(elem));
    int ROW, COL, NZRO = 0;
} Triplet;

void printC(Triplet C) {
    printf("%d\n%d\n%d\n", C.ROW, C.COL, C.NZRO);
    for (int i = 0; i < C.ROW * C.COL; i++) {
        if (C.element[i].data != 0) {
            printf("%d,%d,%d\n", C.element[i].row + 1, C.element[i].col + 1, C.element[i].data);
        }
    }
}

int main() {
    void printC(Triplet);
    Triplet A, B, C;

    scanf("%d %d %d", &A.ROW, &A.COL, &A.NZRO);
    for (int i = 0; i < A.NZRO; i++) {
        scanf("%d %d %d", &A.element[i].row, &A.element[i].col, &A.element[i].data);
        A.element[i].row--;
        A.element[i].col--;
    }

    scanf("%d %d %d", &B.ROW, &B.COL, &B.NZRO);
    for (int i = 0; i < B.NZRO; i++) {
        scanf("%d %d %d", &B.element[i].row, &B.element[i].col, &B.element[i].data);
        B.element[i].row--;
        B.element[i].col--;
    }

    C.ROW = A.ROW;
    C.COL = B.COL;

    for (int i = 0; i < C.ROW; i++) {
        for (int j = 0; j < C.COL; j++) {
            C.element[i * C.COL + j].data = 0;
            C.element[i * C.COL + j].row = i;
            C.element[i * C.COL + j].col = j;
        }
    }

    for (int i = 0; i < A.NZRO; i++) {
        for (int j = 0; j < B.NZRO; j++) {
            int d = 0;
            if (A.element[i].col == B.element[j].row)
                d = A.element[i].data * B.element[j].data;

            if (d != 0) {
                C.element[A.element[i].row * C.COL + B.element[j].col].data += d;
                if (C.element[A.element[i].row * C.COL + B.element[j].col].data == 0)
                    C.NZRO--;
                else if (C.element[A.element[i].row * C.COL + B.element[j].col].data == d)
                    C.NZRO++;
            }
        }
    }

    printC(C);
    return 0;
}
```

# 2. 广义表的建立与基本操作

```
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<cstring>

using namespace std;

typedef enum{ATOM,LIST} Elemtype;
struct GLNode{
    Elemtype tag;
    union{
        char atom;
        struct{struct GLNode *head,*tail;};
    };
};

typedef struct GLNode GList;

void GetSubstr(char substr[], char headstr[]){
    char s[strlen(substr)];
    int i=0;
    int j=0;
    int flag=0;
    int r=0;
    while(substr[i]&&(substr[i]!=','||flag)){
        if(substr[i]=='(')
            flag++;
        else if(substr[i]==')')
            flag--;
        if(substr[i]!=','||(substr[i]==','&&flag)){
            headstr[j]=substr[i];
            j++;
            i++;
        }
    }
    headstr[j]='\0';
    if(substr[i]==',')
        i++;
    while(substr[i]){
        s[r]=substr[i];
        r++;
        i++;
    }
    s[r]='\0';
    strcpy(substr,s);

}

GList *GListcreat(char s[]){
    GList *G;
    GList *p,*q;

    int len = strlen(s);
    char substr[len];
    char headstr[len];

    if(len==0||!strcmp(s,"()")){
        G = NULL;
    }
    else if(len==1){
        G = (GList*)malloc(sizeof(GList));
        if(!G){
            exit(0);
        }
        G->tag=ATOM;
        G->atom = *s;
        G->tail=NULL;

    }
    else{
        G = (GList*)malloc(sizeof(GList));
        if(!G){
            exit(0);
        }
        G->tag=LIST;
        p = G;
        s++;
        strncpy(substr,s,len-2);
        substr[len-2]='\0';
        while(len>0){
            GList *r;
            GetSubstr(substr,headstr);
            r = GListcreat(headstr);
            p->head=r;
            q=p;
            len = strlen(substr);
            if(len>0){
                p = (GList*)malloc(sizeof(GList));
                if(!p){
                    exit(0);
                }
                p->tag=LIST;
                q->tail=p;
            }

        }
        q=p;
        q->tail=NULL;
    }

    return G;
}

void GListPrint(GList *G){
    GList *p,*q;

    if(G&&!G->tag){
        cout<<G->atom;
    }


    else{
        cout<<'(';
        while(G){
            p=G->head;
            q=G->tail;
            if(p&&q&&!p->tag){
                cout<<p->atom<<',';
                p = q->head;
                q=q->tail;
            }

            if(p!=NULL&&!q&&!p->tag){
                cout<<p->atom;
                G = q;
            }
            else{
                if(!p)
                    cout<<"()";
                else
                    GListPrint(p);

                if(q)
                    cout<<',';
                G = q;

            }
        }
        cout<<')';
    }

}

GList *Freelist(GList *G,int n){
    GList *p;
    if(n==1){
        p=G->head;
        G->tail = NULL;
        G = NULL;
        cout<<"destroy tail"<<endl;
        cout<<"free list node"<<endl;

    }
    else if(n==2){
        p = G->tail;
        G->head = NULL;
        G=NULL;
        cout<<"free head node"<<endl;
        cout<<"free list node"<<endl;

    }
    else
        exit(0);

    return p;
}

int main(){
    string assistStr;
    int i;
    char *s;
    GList *G;
    int choose;
    int strlen;

    cin>>assistStr;
    strlen = (signed int) assistStr.size();
    s = (char *)malloc(sizeof(char)*(strlen+1));
    for(i=0;i<(signed int)assistStr.size();i++){
        s[i]=assistStr[i];
    }
    s[strlen]='\0';


    G = GListcreat(s);


    cout<<"generic list: ";
    GListPrint(G);
    cout<<endl;

    while(G&&G->tag){
        cin>>choose;
        switch(choose){
            case 1:
                G = Freelist(G,1);
                cout<<"generic list: ";
                GListPrint(G);
                cout<<endl;
                break;
            case 2:
                G = Freelist(G,2);
                cout<<"generic list: ";
                GListPrint(G);
                cout<<endl;
                break;
            default:
                break;
        }
    }



    return 0;
}
```

# H2. 广义表反序（选做）

```
/* PRESET CODE BEGIN - NEVER TOUCH CODE BELOW */

#include "stdio.h"
#include "string.h"
#include "stdlib.h"

typedef enum { ATOM, LIST } ListTag;

typedef struct node {
    ListTag  tag;
    union {
        char  data;
        struct node *hp;
    } ptr;
    struct node *tp;
} GLNode;

GLNode * reverse( GLNode * );

int count;

void Substring( char *sub, char *s, int pos, int len )
{
    s = s + pos;
    while ( len > 0 )
    {   *sub = *s;
        sub++;
        s++;
        len--;
    }
    *sub = '\0';
}

void sever( char *str, char *hstr )
{   int n, i, k;
    char ch[50];
    n = strlen(str);
    i = k = 0;
    do
    {   Substring( ch, str, i++, 1 );
        if ( *ch=='(' )
            k ++;
        else if ( *ch==')' )
            k --;
    } while ( i<n && ( *ch!=',' || k!=0 ) );

    if ( i<n )
    {   Substring( hstr, str, 0, i-1 );
        Substring( str, str, i, n-i );
    }
    else
    {   strcpy( hstr, str );
        str[0] = '\0';
    }
}  /* sever */

int PrintGList( GLNode * T )
{
    GLNode *p=T, *q;

    if ( p==NULL )
        printf( ")" );
    else
    {   if ( p->tag==ATOM )
        {   if ( count > 0 )
                printf( "," );
            printf( "%c", p->ptr.data );
            count ++;
        }
        else
        {   q = p->ptr.hp;
            if ( q == NULL )
            {   if ( count > 0 )
                    printf(",");
                printf("(");
            }
            else if ( q->tag == LIST )
            {   if ( count > 0 )
                    printf( "," );
                printf( "(" );
                count = 0;
            }
            PrintGList( q );
            PrintGList( p->tp );
        }
    }
    return 1;
}

void print( GLNode * L )
{
    if ( L == NULL )
        printf( "()" );
    else
    {
        if ( L->tag == LIST )
            printf( "(" );
        if ( L->ptr.hp != NULL )
            PrintGList( L );
        else
        {
            printf( "()" );
            if ( L->tp == NULL )
                printf( ")" );
        }
    }
    printf( "\n" );
}

int CreateGList( GLNode **L,  char *s )
{
    GLNode *p, *q;
    char sub[100],  hsub[100];

    p = *L;
    if ( strcmp(s, "()" )==0 )
        *L = NULL;    /* 创建空表 */
    else
    {
        *L = ( GLNode * ) malloc( sizeof( GLNode ) );
        if ( strlen(s)==1 )
        {   (*L)->tag = ATOM;
            (*L)->ptr.data = s[0];
        }
        else
        {   (*L)->tag = LIST;
            p = *L;
            Substring( sub, s, 1, strlen(s)-2 );
            do
            {   sever( sub, hsub );
                CreateGList( &p->ptr.hp, hsub );
                q = p;
                if ( strlen(sub) > 0 )
                {   p = (GLNode *) malloc( sizeof(GLNode) );
                    p->tag = LIST;
                    q->tp = p;
                }
            } while ( strlen(sub)>0 );
            q->tp = NULL;
        }   /* else */
    }  /* else */
    return 1;
}

/**********
这是你要实现的函数。
***********/
GLNode * reverse( GLNode *p );
/*******************/

int main( )
{
    char list[100];
    GLNode *L, *G;
    int d;

    count = 0;
    scanf("%s", list);
    CreateGList( &L, list );

/*  print( L );   */
    G = reverse( L );
    count = 0;
    print( G );
    return 0;
}

/* PRESET CODE END - NEVER TOUCH CODE ABOVE */


GLNode *reverse(GLNode *p) {
    GLNode *MARK = p, *q;

    if (p != NULL && p->tag == LIST) {
        p->tp = reverse(p->tp);

        for (; p->tp != NULL; ) {
            p = p->tp;
        }

        p->tp = (GLNode *)malloc(sizeof(GLNode));
        q = p->tp;
        q->tp = NULL;
        q->tag = MARK->tag;
        q->ptr.hp = reverse(MARK->ptr.hp);
        p->tp = q;
        MARK = MARK->tp;
    } else {
        return MARK; // ATOM
    }

    return MARK;
}
```
