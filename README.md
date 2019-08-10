#include <cstdlib>
#include <iostream>
#include <string>
using namespace std;

#define null NULL

class student
{
    private:
        friend class studentMessage;
        student *next; //节点指针
        string name; //学生姓名
        int age; //年纪
        int id; //学号
        double score[3]; //三科成绩
        double total; //总分
        double average; //平均成绩
    public:
        student(string _name,int _age,int _id,double *_score)
        {
            name = _name;
            age = _age;
            id = _id;
            score[0] = _score[0];
            score[1] = _score[1];
            score[2] = _score[2];
            total = score[0]+score[1]+score[2];
            average = total/3;
            next = NULL;
        }
        student() //为studentMessage初始化头结点用
        {
            name = "null";
            age = 0;
            id = 0;
            score[0]=score[1]=score[2]=0;
            total = 0;
            average = 0;
            next = NULL;
        }
        ~student(){}
        void swap(student*);
};


void student::swap(student *q)
{
    string _name;
    int _age,_id;
    double _score[3],_total,_average;
    
    _name = name;
    name = q->name;
    q->name = _name;
    
    _age = age;
    age = q->age;
    q->age = _age;
    
    _id = id;
    id = q->id;
    q->id = _id;
    
    _score[0] = score[0];
    score[0] = q->score[0];
    q->score[0] = _score[0];
    
    _score[1] = score[1];
    score[1] = q->score[1];
    q->score[1] = _score[1];
    
    _score[2] = score[2];
    score[2] = q->score[2];
    q->score[2] = _score[2];
    
    _total = total;
    total = q->total;
    q->total = _total;
    
    _average = average;
    average = q->average;
    q->average = _average;
}






class studentMessage
{
    private:
      student *first; //头指针
      int num; //信息中的学生人数
    public:
        studentMessage()
        {
            num = 0; //初始化学生人数为0
            first = new student;  //初始化头结点
        }
        ~studentMessage(){delete first;}

        /*管理系统常规操作*/
        void Insret(void); //插入
        void Display(void); //显示
        void Delete(void); //删除
        void Search(void); //搜索
        void Change(void); //改动
        void sortByLesson1(void); //按成绩一来排序
        void sortByLesson2(void); //按成绩二来排序
        void sortByLesson3(void); //按成绩三来排序
        void sortByTotal(void); //按总分来排序
        void SearchByid(void); //按照学号查找
        void SearchByname(void); //按照姓名查找
        int menu(void); //初始的菜单
        void clear(void); //清空列表
};


int studentMessage::menu(void)
{
    int ch;
    cout<<"**********************************************************************"<<endl;
    cout<<"======================================================================"<<endl;
    cout<<"***************************学生成绩管理系统***************************"<<endl;cout<<endl;
    cout<<"1.显示所有学生成绩"<<endl;
    cout<<"2.添加学生信息"<<endl;
    cout<<"3.查询学生信息"<<endl;
    cout<<"4.修改学生信息"<<endl;
    cout<<"5.删除最下面一个学生信息"<<endl;
    cout<<"6.删除所有信息"<<endl;
    cout<<"0.退出系统"<<endl;cout<<endl;
    cout<<"********************Copyright@ By Jeaven Wong**************************"<<endl;
    cout<<"======================================================================="<<endl;
    cout<<"***********************************************************************"<<endl;
    cin >> ch;
    cout<<"\n\n\n"<<endl;
    return ch;
}



//插入
void studentMessage::Insret(void)
{
    string name;
    int age;
    int id;
    double score[3];
    cout<<"请输入学生姓名: ";
    cin>>name;
    cout<<"请输入学生年龄: ";
    cin>>age;
    cout<<"请输入学生学号: ";
    cin>>id;
    cout<<"下面请输入学生三门课程成绩: ";
    cout<<endl;
    cout<<"请输入第一门课的成绩: ";cin>>score[0];
    cout<<"请输入第二门课的成绩: ";cin>>score[1];
    cout<<"请输入第三门课的成绩: ";cin>>score[2];
    cout<<endl;

    
    student *newstu = new student(name,age,id,score);
    student *p = first;
    while(p->next != NULL)
    {
        p = p->next;
    }
    p->next = newstu;
    newstu->next = null;
    num++;
}



void studentMessage::Display(void)
{
    if(num == 0)
    {
        cout<<"当前记录中无学生..."<<endl;
    }
        
    else
    {
        student *p = first->next;
        while(p != null)
        {
            cout<<"姓名:"<<p->name<<"  ";
            cout<<"年龄:"<<p->age<<"  ";
            cout<<"学号:"<<p->id<<"  ";
            cout<<"三门课程成绩: "<<p->score[0]<<" "<<p->score[1]<<" "<<p->score[2]<<"  ";
            cout<<"总分:"<<p->total<<"  ";
            cout<<"平均分:"<<p->average<<endl;
            p = p->next;
        }
    }    
}


void studentMessage::Delete(void)
{
    student *p = first;
    student *pre = first;
    while(p->next != NULL)
    {
        pre = p;
        p = p->next;
    }
    pre->next = NULL;
    delete p;
    num--;
}


void studentMessage::Search(void)
{
    int temp = 0;
    cout<<"请输入查找的条件，有如下选项..."<<endl;
    cout<<"按照学号查找（请输入【1】） 按照姓名查找（请输入【2】）"<<endl;
    cin>>temp;
    switch(temp)
    {
        case 1: SearchByid(); break;
        case 2: SearchByname(); break;
        default: cout<<"输入有误..."<<endl;
    }
}

void studentMessage::SearchByid(void)
{
    int _id;
    int flag = 0;
    cout<<"请输入待查找学生的学号：";
    cin >> _id;
    student *p = first->next;
    while(p != null)
    {
        if(p->id == _id)
        {
            flag = 1;
            cout<<"下面是查找匹配结果："<<endl;
            cout<<"姓名:"<<p->name<<"  ";
            cout<<"年龄:"<<p->age<<"  ";
            cout<<"学号:"<<p->id<<"  ";
            cout<<"三门课程成绩: "<<p->score[0]<<" "<<p->score[1]<<" "<<p->score[2]<<"  ";
            cout<<"总分:"<<p->total<<"  ";
            cout<<"平均分:"<<p->average<<endl;    
        } 
        p = p->next;
    }
    if(flag == 0)
    {
        cout<<"抱歉，记录中没有查找匹配项..."<<endl;
    }
}

void studentMessage::SearchByname(void)
{
    string _name;
    int flag = 0;
    cout<<"请输入待查找的学生姓名: ";
    cin >> _name;
    student *p = first->next;
    while(p != null)
    {
        if(p->name == _name)
        {
            flag = 1;
            cout<<"下面是查找匹配结果："<<endl;
            cout<<"姓名:"<<p->name<<"  ";
            cout<<"年龄:"<<p->age<<"  ";
            cout<<"学号:"<<p->id<<"  ";
            cout<<"三门课程成绩: "<<p->score[0]<<" "<<p->score[1]<<" "<<p->score[2]<<"  ";
            cout<<"总分:"<<p->total<<"  ";
            cout<<"平均分:"<<p->average<<endl;    
        }
        p = p->next;
    }
    
    if(flag == 0)
    {
        cout<<"抱歉，记录中没有查找匹配项..."<<endl;
    }
}



void studentMessage::Change(void)
{
    string _name;
    int flag = 0,temp;
    int _id,_age;
    int course = 0;
    cout<<"请输入需要改动信息的学生的姓名: ";
    cin >> _name;
    student *p = first->next;
    while(p != null)
    {
        if(p->name == _name)
        {
            flag = 1;
            cout<<"该学生当前信息如下："<<endl;
            cout<<"姓名:"<<p->name<<"  ";
            cout<<"年龄:"<<p->age<<"  ";
            cout<<"学号:"<<p->id<<"  ";
            cout<<"三门课程成绩: "<<p->score[0]<<" "<<p->score[1]<<" "<<p->score[2]<<"  ";
            cout<<"总分:"<<p->total<<"  ";
            cout<<"平均分:"<<p->average<<endl;    
            cout<<"请指明哪一项需要修改..."<<endl;
            cout<<"修改学号（输入【1】） 修改年龄（输入【2】）修改成绩（输入【3】）"<<endl;
            cin >> temp;
            switch(temp)
            {
                case 1: 
                    {
                        cout<<"请输入新的学号：";cin>>_id;
                        p->id = _id;
                    }
                    break;
                case 2: 
                    {
                        cout<<"请输入新的年龄：";cin>>_age;
                        p->age = _age;
                    }
                    break;
                case 3: 
                    {
                        cout<<"请按指示修改课程成绩..."<<endl;
                        cout<<"是否需要修改第一门课程成绩？（需要输入【1】不需要输入【0】）"<<endl;
                        cin >> course;
                        if(course == 1)
                        {
                            cout<<"请输入修改后的第一门课的成绩："; cin >> p->score[0];
                        }
                        course = 0;
                        
                        cout<<"是否需要修改第二门课程成绩？（需要输入【1】不需要输入【0】）"<<endl;
                        cin >> course;
                        if(course == 1)
                        {
                            cout<<"请输入修改后的第二门课的成绩："; cin >> p->score[1];
                        }
                        course = 0;
                        
                        cout<<"是否需要修改第三门课程成绩？（需要输入【1】不需要输入【0】）"<<endl;
                        cin >> course;
                        if(course == 1)
                        {
                            cout<<"请输入修改后的第三门课的成绩："; cin >> p->score[2];
                        }
                        course = 0;
                        
                        p->total = p->score[0]+p->score[1]+p->score[2];
                        p->average = p->total/3;
                        
                        cout<<"修改后的信息如下： "<<endl;
                        cout<<"姓名:"<<p->name<<"  ";
                        cout<<"年龄:"<<p->age<<"  ";
                        cout<<"学号:"<<p->id<<"  ";
                        cout<<"三门课程成绩: "<<p->score[0]<<" "<<p->score[1]<<" "<<p->score[2]<<"  ";
                        cout<<"总分:"<<p->total<<"  ";
                        cout<<"平均分:"<<p->average<<endl;    
                    }
                    break;
                default:  cout<<"输入有误..."<<endl;
            }
        }
        p = p->next;
    }
    if(flag == 0)
        cout<<"当前记录中没有次学生..."<<endl;
}

/*排序我均采用冒泡排序法，均是从小到大排序*/


//按照科目一排序
void studentMessage::sortByLesson1(void) 
{
    student *p = first->next;
    while(p->next != null)
    {
        student *q = p->next;
        while(q != null)
        {
            if(p->score[0] > q->score[0])
            {
                p->swap(q);
            }
            q = q->next;
        }
        p = p->next;
    }
}

//按照科目二排序
void studentMessage::sortByLesson2(void)
{
    student *p = first->next;
    while(p->next != null)
    {
        student *q = p->next;
        while(q != null)
        {
            if(p->score[1] > q->score[1])
            {
                p->swap(q);
            }
            q = q->next;
        }
        p = p->next;
    }
}

//按照科目三排序
void studentMessage::sortByLesson3(void)
{
    student *p = first->next;
    while(p->next != null)
    {
        student *q = p->next;
        while(q != null)
        {
            if(p->score[2] > q->score[2])
            {
                p->swap(q);
            }
            q = q->next;
        }
        p = p->next;
    }
}

//按照总成绩排序
void studentMessage::sortByTotal(void)
{
    student *p = first->next;
    while(p->next != null)
    {
        student *q = p->next;
        while(q != null)
        {
            if(p->total > q->total)
            {
                p->swap(q);
            }
            q = q->next;
        }
        p = p->next;
    }
}

void studentMessage::clear(void)
{
    student *p = first->next;
    while(p != null)
    {
        first->next = p->next;
        p->next = null;
        delete p;
        p = first->next;
    }
}



int main()
{
    studentMessage stulist;
    int ch;
    while(ch = stulist.menu())
    {
        switch(ch)
        {
            case 1: stulist.Display(); break;
            case 2: stulist.Insret(); break;
            case 3: stulist.Search(); break;
            case 4: stulist.Change(); break;
            case 5: stulist.Delete(); break;
            case 6: stulist.clear(); break;
            default: cout<<"请按要求输入..."<<endl;
        }
    }
    return 0#include <cstdlib>
#include <iostream>
#include <string>
using namespace std;

#define null NULL

class student
{
    private:
        friend class studentMessage;
        student *next; //节点指针
        string name; //学生姓名
        int age; //年纪
        int id; //学号
        double score[3]; //三科成绩
        double total; //总分
        double average; //平均成绩
    public:
        student(string _name,int _age,int _id,double *_score)
        {
            name = _name;
            age = _age;
            id = _id;
            score[0] = _score[0];
            score[1] = _score[1];
            score[2] = _score[2];
            total = score[0]+score[1]+score[2];
            average = total/3;
            next = NULL;
        }
        student() //为studentMessage初始化头结点用
        {
            name = "null";
            age = 0;
            id = 0;
            score[0]=score[1]=score[2]=0;
            total = 0;
            average = 0;
            next = NULL;
        }
        ~student(){}
        void swap(student*);
};


void student::swap(student *q)
{
    string _name;
    int _age,_id;
    double _score[3],_total,_average;
    
    _name = name;
    name = q->name;
    q->name = _name;
    
    _age = age;
    age = q->age;
    q->age = _age;
    
    _id = id;
    id = q->id;
    q->id = _id;
    
    _score[0] = score[0];
    score[0] = q->score[0];
    q->score[0] = _score[0];
    
    _score[1] = score[1];
    score[1] = q->score[1];
    q->score[1] = _score[1];
    
    _score[2] = score[2];
    score[2] = q->score[2];
    q->score[2] = _score[2];
    
    _total = total;
    total = q->total;
    q->total = _total;
    
    _average = average;
    average = q->average;
    q->average = _average;
}






class studentMessage
{
    private:
      student *first; //头指针
      int num; //信息中的学生人数
    public:
        studentMessage()
        {
            num = 0; //初始化学生人数为0
            first = new student;  //初始化头结点
        }
        ~studentMessage(){delete first;}

        /*管理系统常规操作*/
        void Insret(void); //插入
        void Display(void); //显示
        void Delete(void); //删除
        void Search(void); //搜索
        void Change(void); //改动
        void sortByLesson1(void); //按成绩一来排序
        void sortByLesson2(void); //按成绩二来排序
        void sortByLesson3(void); //按成绩三来排序
        void sortByTotal(void); //按总分来排序
        void SearchByid(void); //按照学号查找
        void SearchByname(void); //按照姓名查找
        int menu(void); //初始的菜单
        void clear(void); //清空列表
};


int studentMessage::menu(void)
{
    int ch;
    cout<<"**********************************************************************"<<endl;
    cout<<"======================================================================"<<endl;
    cout<<"***************************学生成绩管理系统***************************"<<endl;cout<<endl;
    cout<<"1.显示所有学生成绩"<<endl;
    cout<<"2.添加学生信息"<<endl;
    cout<<"3.查询学生信息"<<endl;
    cout<<"4.修改学生信息"<<endl;
    cout<<"5.删除最下面一个学生信息"<<endl;
    cout<<"6.删除所有信息"<<endl;
    cout<<"0.退出系统"<<endl;cout<<endl;
    cout<<"********************Copyright@ By Jeaven Wong**************************"<<endl;
    cout<<"======================================================================="<<endl;
    cout<<"***********************************************************************"<<endl;
    cin >> ch;
    cout<<"\n\n\n"<<endl;
    return ch;
}



//插入
void studentMessage::Insret(void)
{
    string name;
    int age;
    int id;
    double score[3];
    cout<<"请输入学生姓名: ";
    cin>>name;
    cout<<"请输入学生年龄: ";
    cin>>age;
    cout<<"请输入学生学号: ";
    cin>>id;
    cout<<"下面请输入学生三门课程成绩: ";
    cout<<endl;
    cout<<"请输入第一门课的成绩: ";cin>>score[0];
    cout<<"请输入第二门课的成绩: ";cin>>score[1];
    cout<<"请输入第三门课的成绩: ";cin>>score[2];
    cout<<endl;

    
    student *newstu = new student(name,age,id,score);
    student *p = first;
    while(p->next != NULL)
    {
        p = p->next;
    }
    p->next = newstu;
    newstu->next = null;
    num++;
}



void studentMessage::Display(void)
{
    if(num == 0)
    {
        cout<<"当前记录中无学生..."<<endl;
    }
        
    else
    {
        student *p = first->next;
        while(p != null)
        {
            cout<<"姓名:"<<p->name<<"  ";
            cout<<"年龄:"<<p->age<<"  ";
            cout<<"学号:"<<p->id<<"  ";
            cout<<"三门课程成绩: "<<p->score[0]<<" "<<p->score[1]<<" "<<p->score[2]<<"  ";
            cout<<"总分:"<<p->total<<"  ";
            cout<<"平均分:"<<p->average<<endl;
            p = p->next;
        }
    }    
}


void studentMessage::Delete(void)
{
    student *p = first;
    student *pre = first;
    while(p->next != NULL)
    {
        pre = p;
        p = p->next;
    }
    pre->next = NULL;
    delete p;
    num--;
}


void studentMessage::Search(void)
{
    int temp = 0;
    cout<<"请输入查找的条件，有如下选项..."<<endl;
    cout<<"按照学号查找（请输入【1】） 按照姓名查找（请输入【2】）"<<endl;
    cin>>temp;
    switch(temp)
    {
        case 1: SearchByid(); break;
        case 2: SearchByname(); break;
        default: cout<<"输入有误..."<<endl;
    }
}

void studentMessage::SearchByid(void)
{
    int _id;
    int flag = 0;
    cout<<"请输入待查找学生的学号：";
    cin >> _id;
    student *p = first->next;
    while(p != null)
    {
        if(p->id == _id)
        {
            flag = 1;
            cout<<"下面是查找匹配结果："<<endl;
            cout<<"姓名:"<<p->name<<"  ";
            cout<<"年龄:"<<p->age<<"  ";
            cout<<"学号:"<<p->id<<"  ";
            cout<<"三门课程成绩: "<<p->score[0]<<" "<<p->score[1]<<" "<<p->score[2]<<"  ";
            cout<<"总分:"<<p->total<<"  ";
            cout<<"平均分:"<<p->average<<endl;    
        } 
        p = p->next;
    }
    if(flag == 0)
    {
        cout<<"抱歉，记录中没有查找匹配项..."<<endl;
    }
}

void studentMessage::SearchByname(void)
{
    string _name;
    int flag = 0;
    cout<<"请输入待查找的学生姓名: ";
    cin >> _name;
    student *p = first->next;
    while(p != null)
    {
        if(p->name == _name)
        {
            flag = 1;
            cout<<"下面是查找匹配结果："<<endl;
            cout<<"姓名:"<<p->name<<"  ";
            cout<<"年龄:"<<p->age<<"  ";
            cout<<"学号:"<<p->id<<"  ";
            cout<<"三门课程成绩: "<<p->score[0]<<" "<<p->score[1]<<" "<<p->score[2]<<"  ";
            cout<<"总分:"<<p->total<<"  ";
            cout<<"平均分:"<<p->average<<endl;    
        }
        p = p->next;
    }
    
    if(flag == 0)
    {
        cout<<"抱歉，记录中没有查找匹配项..."<<endl;
    }
}



void studentMessage::Change(void)
{
    string _name;
    int flag = 0,temp;
    int _id,_age;
    int course = 0;
    cout<<"请输入需要改动信息的学生的姓名: ";
    cin >> _name;
    student *p = first->next;
    while(p != null)
    {
        if(p->name == _name)
        {
            flag = 1;
            cout<<"该学生当前信息如下："<<endl;
            cout<<"姓名:"<<p->name<<"  ";
            cout<<"年龄:"<<p->age<<"  ";
            cout<<"学号:"<<p->id<<"  ";
            cout<<"三门课程成绩: "<<p->score[0]<<" "<<p->score[1]<<" "<<p->score[2]<<"  ";
            cout<<"总分:"<<p->total<<"  ";
            cout<<"平均分:"<<p->average<<endl;    
            cout<<"请指明哪一项需要修改..."<<endl;
            cout<<"修改学号（输入【1】） 修改年龄（输入【2】）修改成绩（输入【3】）"<<endl;
            cin >> temp;
            switch(temp)
            {
                case 1: 
                    {
                        cout<<"请输入新的学号：";cin>>_id;
                        p->id = _id;
                    }
                    break;
                case 2: 
                    {
                        cout<<"请输入新的年龄：";cin>>_age;
                        p->age = _age;
                    }
                    break;
                case 3: 
                    {
                        cout<<"请按指示修改课程成绩..."<<endl;
                        cout<<"是否需要修改第一门课程成绩？（需要输入【1】不需要输入【0】）"<<endl;
                        cin >> course;
                        if(course == 1)
                        {
                            cout<<"请输入修改后的第一门课的成绩："; cin >> p->score[0];
                        }
                        course = 0;
                        
                        cout<<"是否需要修改第二门课程成绩？（需要输入【1】不需要输入【0】）"<<endl;
                        cin >> course;
                        if(course == 1)
                        {
                            cout<<"请输入修改后的第二门课的成绩："; cin >> p->score[1];
                        }
                        course = 0;
                        
                        cout<<"是否需要修改第三门课程成绩？（需要输入【1】不需要输入【0】）"<<endl;
                        cin >> course;
                        if(course == 1)
                        {
                            cout<<"请输入修改后的第三门课的成绩："; cin >> p->score[2];
                        }
                        course = 0;
                        
                        p->total = p->score[0]+p->score[1]+p->score[2];
                        p->average = p->total/3;
                        
                        cout<<"修改后的信息如下： "<<endl;
                        cout<<"姓名:"<<p->name<<"  ";
                        cout<<"年龄:"<<p->age<<"  ";
                        cout<<"学号:"<<p->id<<"  ";
                        cout<<"三门课程成绩: "<<p->score[0]<<" "<<p->score[1]<<" "<<p->score[2]<<"  ";
                        cout<<"总分:"<<p->total<<"  ";
                        cout<<"平均分:"<<p->average<<endl;    
                    }
                    break;
                default:  cout<<"输入有误..."<<endl;
            }
        }
        p = p->next;
    }
    if(flag == 0)
        cout<<"当前记录中没有次学生..."<<endl;
}

/*排序我均采用冒泡排序法，均是从小到大排序*/


//按照科目一排序
void studentMessage::sortByLesson1(void) 
{
    student *p = first->next;
    while(p->next != null)
    {
        student *q = p->next;
        while(q != null)
        {
            if(p->score[0] > q->score[0])
            {
                p->swap(q);
            }
            q = q->next;
        }
        p = p->next;
    }
}

//按照科目二排序
void studentMessage::sortByLesson2(void)
{
    student *p = first->next;
    while(p->next != null)
    {
        student *q = p->next;
        while(q != null)
        {
            if(p->score[1] > q->score[1])
            {
                p->swap(q);
            }
            q = q->next;
        }
        p = p->next;
    }
}

//按照科目三排序
void studentMessage::sortByLesson3(void)
{
    student *p = first->next;
    while(p->next != null)
    {
        student *q = p->next;
        while(q != null)
        {
            if(p->score[2] > q->score[2])
            {
                p->swap(q);
            }
            q = q->next;
        }
        p = p->next;
    }
}

//按照总成绩排序
void studentMessage::sortByTotal(void)
{
    student *p = first->next;
    while(p->next != null)
    {
        student *q = p->next;
        while(q != null)
        {
            if(p->total > q->total)
            {
                p->swap(q);
            }
            q = q->next;
        }
        p = p->next;
    }
}

void studentMessage::clear(void)
{
    student *p = first->next;
    while(p != null)
    {
        first->next = p->next;
        p->next = null;
        delete p;
        p = first->next;
    }
}



int main()
{
    studentMessage stulist;
    int ch;
    while(ch = stulist.menu())
    {
        switch(ch)
        {
            case 1: stulist.Display(); break;
            case 2: stulist.Insret(); break;
            case 3: stulist.Search(); break;
            case 4: stulist.Change(); break;
            case 5: stulist.Delete(); break;
            case 6: stulist.clear(); break;
            default: cout<<"请按要求输入..."<<endl;
        }
    }
    return 0;
}