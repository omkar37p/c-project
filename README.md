# c-project
student management system
#include<stdio.h>
#include<string.h>
#include<windows.h>
void gotoxy(int x,int y)
{
    COORD c;
    c.X=x;
    c.Y=y;
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE),c);
}
void showbar()
{
    system("color 4f");
    gotoxy(10,12); printf("Roll(U)"); gotoxy(30,12);printf("Roll(C)");
    gotoxy(60,12); printf("NAME");gotoxy(95,12); printf("ADDRESS");
    gotoxy(120,12);printf("Branch");gotoxy(150,12); printf("PHONE");
}
struct student{
  char RU[15],RC[15],name[50],add[30];
  char Branch[4];
  char phone_no[10];
};
struct student stud;

struct account{
  char name[20],pass[8];
};
struct account ac;

void createAdmin()
{
system("cls");
 gotoxy(60,8);printf("CREATE NEW ADMIN");
 char n[20],p[15];
 int isfind=0;
 FILE *fp;
 fp =  fopen("AdminRecord.txt","ab+");
 if(fp==NULL)
 {
     printf("Error in Opening file");
     exit(1);
 }else{
    fflush(stdin);
    gotoxy(50,10);printf("Enter New Admin user name:");gets(n);
    gotoxy(50,12);printf("Enter New Admin user password :");gets(p);
    while(fread(&ac,sizeof(ac),1,fp))
    {
        if(strcmp(n,ac.name)==0)
        {
            isfind=1;
            break;
        }
    }
 }
if(isfind==1){
            gotoxy(49,16);  printf("THE USER IS ALREADY EXISTS");
    }
    else{
            strcpy(ac.name,n);
            strcpy(ac.pass,p);
       fwrite(&ac,sizeof(ac),1,fp);
       gotoxy(49,16);printf("New Admin created successfully");
    }
  getch();
  fclose(fp);
 return;
}
int adminLogin()
{
    system("cls");
    int isfind = 0; char n[20],p[15];
    FILE *fp;
     gotoxy(60,8);printf("LOGIN");
 fp =  fopen("AdminRecord.txt","rb");
   if(fp==NULL)
 {
    gotoxy(50,10); printf("Error in Opening file");
     exit(1);
 }
    fflush(stdin);
    gotoxy(10,10);printf("Enter Admin user name:");gets(n);
    gotoxy(10,12);printf("Enter Admin user password :");gets(p);

    while(fread(&ac,sizeof(ac),1,fp))
    {
        if(strcmp(n,ac.name)==0 && strcmp(p,ac.pass)==0)
        {
            isfind=1;
            break;
        }
    }
    fclose(fp);
    return isfind;
}
void UserWind()
{ int choice,found;

    while(1)
    {
        system("cls");
    gotoxy(58,12);printf("DESCENDANTS OF THE SUN ");
    gotoxy(65,16);printf("1.Create New Admin ");
    gotoxy(65,18);printf("2.Login ");
    gotoxy(65,20);printf("3.Exit ");
    gotoxy(65,22);printf("Enter your choice:");
    scanf("%d",&choice);
    switch(choice)
    {
    case 1:
       createAdmin();
        break;
    case 2:
         found = adminLogin();
         if(found==1)
              menu_win();
         else{
            gotoxy(10,24);printf("Wrong User name / password:");
            getch();
         }
        break;
    case 3:
        gotoxy(66,24);printf("**** THANK YOU *****");
        getch();
        exit(0);
    default:
        gotoxy(60,24);printf("Invalid input,Try again");
    }
  }
  return;
}
void add_student()
{
 system("cls");
 gotoxy(40,8);printf("ADD RECORD");
 FILE *fp;
 fp =  fopen("record.txt","ab+");

 if(fp==NULL)
 {
     printf("Error in Opening file");
 }else{

   fflush(stdin);
   gotoxy(37,10); printf("University Roll : ");gets(stud.RU);
   gotoxy(37,12); printf("College Roll : ");gets(stud.RC);
   gotoxy(37,14); printf("Name : ");gets(stud.name);
   gotoxy(37,16); printf("Address : ");gets(stud.add);
   gotoxy(37,18); printf("Branch : ");gets(stud.Branch);fflush(stdin);
   gotoxy(37,20);  printf("Phone Number : ");gets(stud.phone_no);

   fwrite(&stud,sizeof(stud),1,fp);
   gotoxy(40,22);printf("The record is successfully added");
 }
  fclose(fp);
  return;
}
void SearchMenu()
{  int ch;
while(1){
    system("cls");
    gotoxy(50,10);printf("Search Menu List");
    gotoxy(52,12);printf("1. Roll(U)");
    gotoxy(52,14);printf("2. Name");
    gotoxy(52,16);printf("3. Address");
    gotoxy(52,18);printf("4. Back");
    gotoxy(50,20);printf("Enter your choice : ");
    scanf("%d",&ch);
    switch(ch)
    {
    case 1:
        search_student_R();
        break;
    case 2:
        search_student_Name();
        break;
    case 3:
        search_student_add();
        break;
    case 4:
        menu_win();
        break;
    default:
        gotoxy(50,24);printf("Sorry ! try again");
    }
}
}
void search_student_R()
{
    system("cls");

    char id[15];
    int isfind=0;
     FILE *fp;
   gotoxy(80,8); printf("Search Record");
    gotoxy(40,10);printf("Enter University Roll to search:");fflush(stdin);
    gets(id);

    fp =  fopen("record.txt","rb+");
    while(fread(&stud,sizeof(stud),1,fp))
    {
        if(strcmp(id,stud.RU)==0)
        {
            isfind=1;
            break;
        }
    }
    if(isfind==1)
    {
        gotoxy(50,14);printf("University Roll: %s",stud.RU);
        gotoxy(50,15);printf("College Roll: %s",stud.RC);
        gotoxy(50,16);printf("Name: %s",stud.name);
        gotoxy(50,17);printf("Address: %s",stud.add);
        gotoxy(50,18);printf("Branch: %s",stud.Branch);
        gotoxy(50,19);printf("Phone No: %s",stud.phone_no);
    }
    else{
        gotoxy(37,12);printf("Sory, No record found in the database");}
        getch();
    fclose(fp);
    return;
}
void search_student_Name()
{
    system("cls");

    char name[20];
    int isfind=0;
     FILE *fp;
   gotoxy(80,8); printf("Search Record");
    gotoxy(60,10);printf("Enter Name to search:");fflush(stdin);
    gets(name);

    fp =  fopen("record.txt","rb+");
    while(fread(&stud,sizeof(stud),1,fp))
    {
        if(strcmp(name,stud.name)==0)
        {
            isfind=1;
            break;
        }
    }
    if(isfind==1)
    {
        gotoxy(50,14);printf("University Roll: %s",stud.RU);
        gotoxy(50,15);printf("College Roll: %s",stud.RC);
        gotoxy(50,16);printf("Name: %s",stud.name);
        gotoxy(50,17);printf("Address: %s",stud.add);
        gotoxy(50,18);printf("Branch: %s",stud.Branch);
        gotoxy(50,19);printf("Phone No: %s",stud.phone_no);
    }
    else{
        gotoxy(37,12);printf("Sory, No record found in the database");}
        getch();
    fclose(fp);
    return;
}
void search_student_add()
{
    system("cls");

    char ad[20];
    int i=0;
     FILE *fp;
    gotoxy(60,10);printf("Enter Address to search:");fflush(stdin);
    gets(ad);

    fp =  fopen("record.txt","rb+");
showbar();
    while(fread(&stud,sizeof(stud),1,fp))
    {
        if(strcmp(ad,stud.add)==0)
        {
     gotoxy(10,14+i); printf("%s",stud.RU);
       gotoxy(30,14+i); printf("%s",stud.RC);
       gotoxy(55,14+i); printf("%s",stud.name);
       gotoxy(95,14+i); printf("%s",stud.add);
       gotoxy(120,14+i); printf("%s",stud.Branch);
       gotoxy(150,14+i); printf("%s",stud.phone_no);
        i++;
        }
    }
        getch();
    fclose(fp);
    return;
}
void ShowAllStudent()
{
    system("color 6f");
    system("cls");
     FILE *fp;
     int i=0;
  showbar();
    fp =  fopen("record.txt","rb");

    while(fread(&stud,sizeof(stud),1,fp)>0)
    {
       gotoxy(10,14+i); printf("%s",stud.RU);
       gotoxy(30,14+i); printf("%s",stud.RC);
       gotoxy(55,14+i); printf("%s",stud.name);
       gotoxy(94,14+i); printf("%s",stud.add);
       gotoxy(122,14+i); printf("%s",stud.Branch);
       gotoxy(150,14+i); printf("%s",stud.phone_no);
        i++;

    }
    gotoxy(70,8);printf("TOTAL NUMBER OF STUDENT : %d",i);
        getch();
    fclose(fp);
    return;
}
void mod_student()
{
    FILE *fp;
    system("cls");

    char id[15];
   int isfind = 0;
   gotoxy(40,8); printf("MODIFY STUDENT");
   gotoxy(37,10);printf("Enter College Roll to modify record:");fflush(stdin);
     gets(id);

     fp = fopen("record.txt","rb+");
     while(fread(&stud,sizeof(stud),1,fp)==1)
     {
         if(strcmp(id,stud.RC)==0)
         {
              fflush(stdin);
   gotoxy(37,12); printf("University Roll : ");gets(stud.RU);
   gotoxy(37,14); printf("College Roll : ");gets(stud.RC);


   gotoxy(37,16); printf("Name : ");gets(stud.name);
   gotoxy(37,18); printf("Address : ");gets(stud.add);
   gotoxy(37,20); printf("Branch : ");gets(stud.Branch);fflush(stdin);
   gotoxy(37,22);  printf("Phone Number : ");gets(stud.phone_no);
            fseek(fp,-sizeof(stud), SEEK_CUR);
            fwrite(&stud,sizeof(stud), 1, fp);
            isfind = 1;
            break;
         }
     }
        if(!isfind){
        gotoxy(37, 12);printf("No Record Found");
    }
    getch();
    fclose(fp);
    return;
}
void delete_student()
{
   system("cls");
   char  id[15];
   FILE *fp,*temp;
   fp = fopen("record.txt","rb");
    temp = fopen("temp.txt","wb");

   gotoxy(37,12);printf("Enter College Roll to delete record:");fflush(stdin);
   gets(id);
   if(fp==NULL)
   {
       printf("unable to open  file:");
       exit(1);
   }
   else{
      while(fread(&stud, sizeof(stud),1,fp) == 1){
        if(strcmp(id,stud.RC) != 0){
            fwrite(&stud,sizeof(stud),1,temp);
        }
    }
   }
   fclose(fp);
   fclose(temp);
   remove("record.txt");
   rename("temp.txt","record.txt");

   gotoxy(37,14);printf("The record is successfully deleted");
   getch();
   return;
}

void menu_win()
{
    system("color 4f");
  int choice;
   int x=50;
   while(1)
   {
       system("cls");
       gotoxy(60,10);printf("STUDENT DESHBOARD");
        gotoxy(x,12);printf("1. Add Student");
        gotoxy(x,14);printf("2. Search Student");
        gotoxy(x,16);printf("3. Show all Student Record");
        gotoxy(x,18);printf("4. Modify Student Record");
        gotoxy(x,20);printf("5. Delete Student Record");
        gotoxy(x,22);printf("6. Exit");
        gotoxy(x,24);printf("Enter your choice: ");
        scanf("%d",&choice);
        switch(choice){
            case 1:
                add_student();
                break;
            case 2:
                SearchMenu();
                break;
            case 3:
                ShowAllStudent();
                break;
           case 4:
                mod_student();
                break;
            case 5:
                 delete_student();
                break;
            case 6:
                gotoxy(60,26);printf("**** THANK YOU *****");
                getch();
                exit(0);
                break;
            default:
                break;
        }
   }
}

int main()
{
    system("color 5f");
    UserWind();
}
