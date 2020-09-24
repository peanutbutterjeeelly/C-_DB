//HW3
//Due: 11:59PM, September 25 (Friday)

#include <iostream>
#include <list>
#include <map>
#include <string>
#include <tuple>
#include <iomanip>
using namespace std;

class course {
public:
    string name;
    int section;
    int credits;
    string grade;
    course() {}
    course(string n, int s, int c, string g) { name = n; section = s; credits = c; grade = g; }
//    bool f1(const course &a, const course &b) { return a.name<b.name; }
    //You might need to implement some overloaded operators here.
    bool operator<(const course &c) const {//overload operator <
        auto res = name<c.name;
        return res;
    }
    bool operator==(const course&c) const { return name==c.name;}
};
bool f1(const course *a,const course *b){return *a<*b;}
//in the list are pointer to course objects, as we actually want to sort pointers, so that what we passed in has to be pointers instead of
//reference to objects or objects themself. Then compare, when compare, we can overload operator < or

//Implement the following functions.
//When adding a student, if the student is already in DB, then ignore the operation.
//When adding a course, if the course is already in DB, then ignore the operation.
//When dropping a course, if the course does not exist, then ignore the operation.
//When removing a student, if the student does not exist, then ignore the operation.
//All courses in a semester need to be sorted.
//When dropping or adding a course, overall GPA, semester GPA, overall credits and semester credits all need to be updated.

//Semester numbers:  Spring 2019: 20191; Fall 2019: 20192, etc.
ostream& operator<<(ostream&str,const list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB ){
    str << "DB: "<<endl;
    for(auto i:DB){
        str << "ID: "<< get<0>(i) <<endl;
        str <<"Overall GPA: "<<get<2>(i)<<setprecision(3)<<endl;
        str << "Overall Credits: "<< get<1>(i) <<endl;
        //str << get<3>(i)<<endl;
       // if (get<3>(i)== nullptr)  break;
        for (auto mi:*get<3>(i)){
            if(&mi.first== nullptr||&mi.second== nullptr) break;
            str<<"Semester: "<<mi.first<<endl;
            str<<"Semester_GPA: "<<mi.second->first->second<<endl;
            str<<"Semester_credits: "<<mi.second->first->first<<endl;
            for (auto li: *mi.second->second){
                str <<"(" <<li->name<<" "<< li->section <<" "<<li->credits<<" " << li->grade << ") ";
            }
            str << endl;

        }

    }
    return str;

}
void add_student(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int id);
void remove_student(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int id);
void add_course(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int semester, int id, course c); //20171 Spring semester of 2017; 20172: Fall semester of 2017
//All courses in the list should be sorted according to name (increasing order)
void drop_course(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int semester, int id, course c);
void print_student_semester_courses(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int semester, int id);
void print_student_all_courses(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int id);

//Implement additional functions such that you can do
//cout << DB << endl;

//You might need to implement some overloaded operators in the course class.

int main() {


    list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*> *> *> > DB;

    add_student(DB, 11111);
    course C1("CIS554", 1, 3, "A-"), C2("CSE674", 1, 3, "B+"), C3("MAT296", 8, 4, "A"), C4("WRT205", 5, 3, "A");
    add_course(DB, 20171, 11111, C2);
    add_course(DB, 20171, 11111, C1);
    add_course(DB, 20171, 11111, C4);
    add_course(DB, 20171, 11111, C3);

    print_student_semester_courses(DB, 20171, 11111);

    drop_course(DB, 20171, 11111, C1);
    drop_course(DB, 20171, 11111, C1);

    print_student_semester_courses(DB, 20171, 11111); //sorted according to course name

    course C5("CIS351", 2, 3, "A-"), C6("PSY205", 5, 3, "B+"), C7("MAT331", 2, 3, "A"), C8("ECN203", 4, 3, "A");
    add_course(DB, 20172, 11111, C5);
    add_course(DB, 20172, 11111, C6);
    add_course(DB, 20172, 11111, C7);
    add_course(DB, 20172, 11111, C8);
    add_course(DB, 20172, 11111, C3);
   print_student_all_courses(DB, 11111);//ID GPA

    add_student(DB, 11112);
    add_course(DB, 20171, 11112, C2);
    add_course(DB, 20171, 11112, C5);
    add_course(DB, 20171, 11112, C7);
    add_course(DB, 20171, 11112, C4);
    print_student_semester_courses(DB, 20171, 11112);

    add_course(DB, 20172, 11112, C8);
    add_course(DB, 20172, 11112, C3);
    add_course(DB, 20172, 11112, C5);
    add_course(DB, 20172, 11112, C1);
    print_student_semester_courses(DB, 20172, 11112);

    print_student_all_courses(DB, 11112);

    cout << DB << endl;
    remove_student(DB, 11111);
    cout << DB << endl;
    getchar();
    getchar();
    return 0;
}

void add_student(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int id) {

    for (auto ListIterator: DB) {
        if (id == get<0>(ListIterator)) {
            return;
        }
    }
        tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*> *> *> T1;//initialize a tuple here
         //get<0>(T1) = id;
        // DB.push_back(T1);
        T1 = make_tuple(id,0,0.0,new map<int, pair<pair<int, float>*,list<course*>*>*>);
        DB.push_back(T1);

}
void remove_student(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int id) {
    //map<int, pair<pair<int,float>,list<course>>> M1;
    //DB<tuple>::iterator ListI;


    for(auto TupleIterator=DB.begin();TupleIterator!=DB.end();){
        if(get<0>(*TupleIterator)==id){
            map<int, pair<pair<int, float>*, list<course*>*>*>& mapping = *get<3>(*TupleIterator);//store the map to &mapping by reference
            if(mapping.size()==0){//if this map is empty, we directly remove this tuple, this struc is not created thru new
            DB.erase(TupleIterator);
            TupleIterator++;}
            else{
                for(auto &j:mapping){
                    if(j.second==nullptr) continue;
                    pair<pair<int, float>*, list<course*>*>& bb=*mapping[j.first];
                    list<course*> cc= *bb.second;
                    auto k = cc.begin();
                    while(k!=cc.end()){
                        cc.erase(k++);
                        if(cc.size()==0) break;
                        //erase all of the course
                    }

                    delete bb.second;//delete pointer to course list
                    bb.second= nullptr;
                    delete bb.first;//delete pointer to credit and GPA pair
                    bb.first= nullptr;
                    delete j.second;//delete pointer to pair
                    j.second= nullptr;
//                    delete get<3>(*TupleIterator);
//                    get<3>(*TupleIterator)= nullptr;

                }
                DB.erase(TupleIterator);
                TupleIterator++;

        }
        TupleIterator++;
    }
}
}


void add_course(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int semester, int id, course c) {
        for(auto Iterator = DB.begin();Iterator!=DB.end();Iterator++){
            if(get<0>(*Iterator)==id){
                //add course
                //get<3>(*Iterator)->insert(course);

                map<int, pair<pair<int, float>*, list<course*>*>*>& mapping = *get<3>(*Iterator);
                //map<int, pair<pair<int, float>*, list<course*>*>*>::iterator Mapping;
                for(auto i:mapping){
                    for(auto j:*i.second->second){
                        if(j->name==c.name) return;//when id hit, we dont want add repeated course
                    }
                }
                if (mapping[semester]== nullptr){//when one semester has no course, directly push this course
                    list<course*>* C = new list<course*>;
                    C ->push_back(&c);
                    pair<pair<int,float>*,list<course*>*>* Pair1 = new pair<pair<int,float>*,list<course*>*>;
                    pair<int,float>* Pair2 = new pair<int,float>;//when there is no course, no corresponding data structure, create new
                    Pair2->first = c.credits;//assign this new course credit to the semester credit and GPA
                    if (c.grade=="A"){
                        Pair2->second = 4.0;
                    }
                    else if (c.grade=="A-"){
                        Pair2->second = 3.7;
                    }
                    else if (c.grade=="B+"){
                        Pair2->second= 3.3;
                    }
                    else if (c.grade=="B"){
                        Pair2->second =3.0;
                    }
                    else if(c.grade == "B-"){
                        Pair2->second=2.7;
                    }
                    else if(c.grade == "C+"){
                        Pair2->second=2.3;
                    }
                    else if(c.grade == "C"){
                        Pair2->second=2.0;
                    }
                    else if(c.grade == "D+"){
                        Pair2->second = 1.3;
                    }
                    else if(c.grade == "F"){
                        Pair2->second= 0.0;
                    }
                    Pair1->first = Pair2;
                    Pair1->second=C;
                    mapping[semester]=Pair1;
                }

                //else if (mapping[semester]->second)
                else{// there has been existed data structure, push in
                    mapping[semester]->second->push_back(&c);//push this course in the list
                    mapping[semester]->second->sort(f1);

                    int denominator = 0;
                    float fraction = 0;
                    float score=0;
                    float GPA = 0;
                    for(auto i: *mapping[semester]->second){//iterate the list

                        if (i->grade=="A"){
                            score = 4.0;
                        }
                        else if (i->grade=="A-"){
                            score= 3.7;
                        }
                        else if (i->grade=="B+"){
                            score= 3.3;
                        }
                        else if (i->grade=="B"){
                            score =3.0;
                        }
                        else if(i->grade == "B-"){
                            score =2.7;
                        }
                        else if(i->grade == "C+"){
                            score =2.3;
                        }
                        else if(i->grade == "C"){
                            score =2.0;
                        }
                        else if(i->grade == "D+"){
                            score =1.3;
                        }
                        else if(i->grade == "F"){
                            score= 0.0;
                        }
                        denominator+=i->credits;//add all credits
                        fraction+=i->credits*score;//adds up as fraction
                        GPA = fraction/denominator;//calculate each semester's GPA
                    }
                    mapping[semester]->first->first= denominator;//store to pair<pair, first pos is credits, which is denominator
                    mapping[semester]->first->second=GPA;//store
                }
                float semesterFraction = 0;
                int semesterCredits = 0;
                float totalGPA = 0;
                for (auto i: mapping){//traversal the map
                    semesterCredits+=i.second->first->first;//add up, it's second pos of map, then first first
                    semesterFraction+=i.second->first->second*i.second->first->first;//adds up the multiply
                }
                totalGPA = semesterFraction/semesterCredits;
                get<2>(*Iterator) = totalGPA;
                get<1>(*Iterator) = semesterCredits;
            }
        }
}

void drop_course(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int semester, int id, course c) {
//    int semesterCredit = 0;
//    float semesterFraction = 0;
      float score=0;
//    float semesterGPA = 0;
      int removecredit = 0;
      float removescore = 0;
      float newSemesterGPA = 0;
      int newSemesterCredit = 0;
      int newCredit = 0;
      float newFraction = 0;
      float newGPA = 0;
      float newSemesterFraction = 0;
      for(auto& i:DB){
          if(get<0>(i)==id){
              map<int, pair<pair<int, float>*, list<course*>*>*> &aa = *get<3>(i);
              for(auto &j:aa){
                  pair<pair<int, float>*, list<course*>*>& bb=*aa[semester];
                  list<course*>& cc= *bb.second;
                  for(auto k=cc.begin();k!=cc.end();){
                      if((*k)->name==c.name){
                          removecredit=(*k)->credits;
                          if ((*k)->grade=="A"){
                              score = 4.0;
                          }
                          else if ((*k)->grade=="A-"){
                              score = 3.7;
                          }
                          else if ((*k)->grade=="B+"){
                              score= 3.3;
                          }
                          else if ((*k)->grade=="B"){
                              score =3.0;
                          }
                          else if((*k)->grade == "B-"){
                              score=2.7;
                          }
                          else if((*k)->grade == "C+"){
                              score=2.3;
                          }
                          else if((*k)->grade == "C"){
                              score=2.0;
                          }
                          else if((*k)->grade == "D+"){
                              score = 1.3;
                          }
                          else if((*k)->grade== "F"){
                              score= 0.0;
                          }
                          removescore =score;
                          //delete *k;
                          cc.erase(k++);

                      }
                      else k++;
                  }
                  if(cc.size()==0){//if this semester's course is empty, we have to delete data in pair and corresponding map[semester]
                      delete bb.second;//delete data in pair
                      bb.second= nullptr;
                      delete bb.first;
                      bb.first= nullptr;
                      delete j.second;
                      j.second= nullptr;
                      for(auto h=aa.begin();h!=aa.end();){
                          if (h->first == semester)
                              aa.erase(h++);//do it in map when hit semester
                          else{h++;}
                      }
                      break;//this time needs to break, or there would be error when we add one and delete one course.


                  }
                  else{
                      //calculate GPA here
                      newSemesterCredit = bb.first->first-removecredit;
                      newSemesterFraction = bb.first->first*bb.first->second-removescore*removecredit;
                      newSemesterGPA = newSemesterFraction/newSemesterCredit;
                      bb.first->first = newSemesterCredit;
                      bb.first->second = newSemesterGPA;
                      for(auto &m:aa){
                          newCredit+=m.second->first->first;
                          newFraction+= m.second->first->first*m.second->first->second;
                          newGPA = newFraction/newCredit;
                      }

                  }
              }
              get<1>(i)=newCredit;
              //cout << newCredit << " "<<newFraction;
              get<2>(i)=newGPA;

          }

      }




//    for(auto i = DB.begin();i!=DB.end();i++){
//                if(get<0>(*i)==id&&get<3>(*i)!= nullptr){
//                    for (auto m:*get<3>(*i)){
//                        if (m.first == semester&&m.second->second!= nullptr){
//
//
//
//
//                           // auto li = *m.second->second->begin();
//                            for(auto li=*m.second->second->begin();li!=*m.second->second->end();) {
//                              //while(li!=*m.second->second->end()){
//                                if (li->name == c.name) {
//                                    removecredit = c.credits;
//                                    if (li->grade == "A") {
//                                        score = 4.0;
//                                    } else if (li->grade == "A-") {
//                                        score = 3.7;
//                                    } else if (li->grade == "B+") {
//                                        score = 3.3;
//                                    } else if (li->grade == "B") {
//                                        score = 3.0;
//                                    } else if (li->grade == "B-") {
//                                        score = 2.7;
//                                    } else if (li->grade == "C+") {
//                                        score = 2.3;
//                                    } else if (li->grade == "C") {
//                                        score = 2.0;
//                                    } else if (li->grade == "D+") {
//                                        score = 1.3;
//                                    } else if (li->grade == "F") {
//                                        score = 0.0;
//                                    }
//                                    removescore = score;
//                                    newSemesterCredit = m.second->first->first-c.credits;
//                                    newSemesterGPA = ((m.second->first->first*m.second->first->second)-(removescore*removecredit))/newSemesterCredit;
//                                    m.second->first->first = newSemesterCredit;
//                                    m.second->first->second = newSemesterGPA;
//                                   // m.second->second->erase(li,);
//                                    delete li;
//                                    li++;
//
//
//                                    break;
//                                }
//                                li++;
////                                for (auto cal:*m.second->second) {
////
////                                    if (cal->grade == "A") {
////                                        score = 4.0;
////                                    } else if (cal->grade == "A-") {
////                                        score = 3.7;
////                                    } else if (cal->grade == "B+") {
////                                        score = 3.3;
////                                    } else if (cal->grade == "B") {
////                                        score = 3.0;
////                                    } else if (cal->grade == "B-") {
////                                        score = 2.7;
////                                    } else if (cal->grade == "C+") {
////                                        score = 2.3;
////                                    } else if (cal->grade == "C") {
////                                        score = 2.0;
////                                    } else if (cal->grade == "D+") {
////                                        score = 1.3;
////                                    } else if (cal->grade == "F") {
////                                        score = 0.0;
////                                    }
////                                    semesterFraction += cal->credits * score;
////                                    semesterCredit += cal->credits;
////                                    semesterGPA = semesterFraction / semesterCredit;
////                                    m.second->first->first = semesterCredit;
////                                    m.second->first->second = semesterGPA;
////                                }
//                            }
//                        }
//                        newCredit+= m.second->first->first;
//                        newFraction+= m.second->first->first*m.second->first->second;
//                        newGPA = newFraction/newCredit;
//
//
//
//                    }
//                    get<1>(*i)=newCredit;
//                    //cout << newCredit << " "<<newFraction;
//                    get<2>(*i)=newGPA;
//                }
//            }
}

void print_student_semester_courses(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int semester, int id) {

    for (auto i: DB) {
        if (get<0>(i) == id&&get<3>(i)!= nullptr) {//student id hit and its map is not empty, to avoid print nullptr
            cout << "Student ID: "<<id << endl;
            for (auto m:*get<3>(i)) {//iterate in map
                if (m.first == semester) {//#semester hit
                    cout << "Semester: "<< semester << endl;
                    map<int, pair<pair<int, float> *, list<course *> *> *> &mapping = *get<3>(i);
                    if (mapping[semester] == nullptr || &mapping == nullptr) break;
                    else if(mapping[semester]->second!= nullptr) {//make sure inside the list it's not empty
                        for (auto i:*mapping[semester]->second) {//in the worst case, we may drop all of the course and print
                            cout << "(" << i->name << " " << i->section << " " << i->credits << " " << i->grade << ") ";
                        }
                    }
                    cout << endl;
                }
            }
        }
    }

}
void print_student_all_courses(list<tuple<int, int, float, map<int, pair<pair<int, float>*, list<course*>*>*>*> >& DB, int id) {
          for (auto i:DB){
              if(get<0>(i)==id&&get<3>(i)!= nullptr){//be sure to check map
                  cout<< "Student id: "<<id <<endl;
                  for(auto m:*get<3>(i)){
                      cout << "Semester: "<< m.first<<endl;
                      if(m.second->second!= nullptr) {//make sure that course list is not empty
                          for (auto l:*m.second->second) {
                              cout << "(" << l->name << " " << l->section << " " << l->credits << " " << l->grade
                                   << ") ";
                          }
                          cout << endl;
                      }
                  }
              }
          }
}


