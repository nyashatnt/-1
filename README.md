##include <iostream>  
#include <fstream>   
#include <string>   
#include <vector>    
#include <cstdlib>  
#include <algorithm> 
using namespace std;  
class PhonePublic {
public:  
    string surname;     
    string name;      
    string patronymic;
    string address;    
    string number;      
    int local_time;     
    int long_distance_time; 
    int total_time() {
        return local_time + long_distance_time;
    }
    void show() {
        cout << surname << " " << name << " " << patronymic << " | "
            << address << " | " << number << " | "
            << "Local: " << local_time << " | "
            << "Long: " << long_distance_time << " | "
            << "Total: " << total_time() << endl;
    }
};
class PhonePrivate {
private: 
    string surname;
    string name;
    string patronymic;
    string address;
    string number;
    int local_time;
    int long_distance_time;

public: 
    void set_surname(string s) { surname = s; }
    void set_name(string n) { name = n; }
    void set_patronymic(string p) { patronymic = p; }
    void set_address(string a) { address = a; }
    void set_number(string num) { number = num; }
    void set_local_time(int lt) { local_time = lt; }
    void set_long_distance_time(int ldt) { long_distance_time = ldt; }
    string get_surname() { return surname; }
    string get_name() { return name; }
    string get_patronymic() { return patronymic; }
    string get_address() { return address; }
    string get_number() { return number; }
    int get_local_time() { return local_time; }
    int get_long_distance_time() { return long_distance_time; }

    int total_time() {
        return local_time + long_distance_time;
    }
    void show() {
        cout << surname << " " << name << " " << patronymic << " | "
            << address << " | " << number << " | "
            << "Local: " << local_time << " | "
            << "Long: " << long_distance_time << " | "
            << "Total: " << total_time() << endl;
    }
};
int main() {
    vector<PhonePublic> pub;   
    vector<PhonePrivate> priv; 

    ofstream f1("phones.txt"); 
    f1 << "Ivanov, Ivan Ivanovich, Moscow, Lenin Street 10,123-45-67,120,30\n"
        << "Petrov, Petr, Petrovich, SPb Nevsky Pr.5,234-56-78,50,10\n"
        << "Sidorov, Sidor, Sidorovich, Kazan, Baumana Street 15,345-67-89,200,0\n"
        << "Smirnov, Alexey, Ivanovich, Moscow, Mira Avenue 20,456-78-90,80,20\n"
        << "Kuznetsov, Nikolai, Petrovich, Tver, Sovetskaya Street 3,567-89-01,30,5\n";
    f1.close(); 
    ifstream f2("phones.txt"); // открываем файл для чтения
    string line;  
    while (getline(f2, line)) {
        int p1 = line.find(',');           
        int p2 = line.find(',', p1 + 1);   
        int p3 = line.find(',', p2 + 1);   
        int p4 = line.find(',', p3 + 1);   
        int p5 = line.find(',', p4 + 1);   
        int p6 = line.find(',', p5 + 1);  
        string s = line.substr(0, p1);     
        string n = line.substr(p1 + 1, p2 - p1 - 1);             
        string patr = line.substr(p2 + 1, p3 - p2 - 1);         
        string addr = line.substr(p3 + 1, p4 - p3 - 1);         
        string num = line.substr(p4 + 1, p5 - p4 - 1);          
        int lt = atoi(line.substr(p5 + 1, p6 - p5 - 1).c_str());   
        int ldt = atoi(line.substr(p6 + 1).c_str());               

        //объект первого класса
        PhonePublic wp;          
        wp.surname = s;        
        wp.name = n;              
        wp.patronymic = patr;      
        wp.address = addr;         
        wp.number = num;         
        wp.local_time = lt;       
        wp.long_distance_time = ldt; 
        pub.push_back(wp);         

        //объект второго класса
        PhonePrivate wp2;           
        wp2.set_surname(s);     
        wp2.set_name(n);          
        wp2.set_patronymic(patr);  
        wp2.set_address(addr);    
        wp2.set_number(num);        
        wp2.set_local_time(lt);   
        wp2.set_long_distance_time(ldt);
        priv.push_back(wp2);       
    }
    f2.close();  //закрываем файл

    //переменные для ввода пользователя
    int threshold;      //порог для внутригородских разговоров
    cout << "=== PHONE PUBLIC ===" << endl;  
    cout << "Enter local time threshold: ";
    cin >> threshold;    

    //вывод для первой реализации
    cout << "\nAll subscribers:" << endl; 
    for (int i = 0; i < pub.size(); i++)  
        pub[i].show();            

    //а) абоненты с внутригородским временем больше заданного
    cout << "\na) Local time > " << threshold << ":" << endl;
    for (int i = 0; i < pub.size(); i++)
        if (pub[i].local_time > threshold)  
            pub[i].show();                 

    //б) абоненты, воспользовавшиеся междугородней связью
    cout << "\nb) Used long-distance connection:" << endl;
    for (int i = 0; i < pub.size(); i++)
        if (pub[i].long_distance_time > 0) 
            pub[i].show();              

    //в) сортировка по фамилии (алфавитный порядок)
    cout << "\nc) Alphabetical order:" << endl;
    for (int i = 0; i < pub.size() - 1; i++) {
        for (int j = 0; j < pub.size() - i - 1; j++) {
            if (pub[j].surname > pub[j + 1].surname) {
                PhonePublic temp = pub[j];
                pub[j] = pub[j + 1];
                pub[j + 1] = temp;
            }
        }
    }
    for (int i = 0; i < pub.size(); i++)
        pub[i].show();

    //вывод для привата 
    cout << "\n=== PHONE PRIVATE ===" << endl;
    cout << "\nAll subscribers:" << endl;  
    for (int i = 0; i < priv.size(); i++)
        priv[i].show();

    //а) абоненты с внутригородским временем больше заданного
    cout << "\na) Local time > " << threshold << ":" << endl;
    for (int i = 0; i < priv.size(); i++)
        if (priv[i].get_local_time() > threshold)  
            priv[i].show();

    //б) абоненты, воспользовавшиеся междугородней связью
    cout << "\nb) Used long-distance connection:" << endl;
    for (int i = 0; i < priv.size(); i++)
        if (priv[i].get_long_distance_time() > 0) 
            priv[i].show();

    // в) сортировка по фамилии (алфавитный порядок)
    cout << "\nc) Alphabetical order:" << endl;
    for (int i = 0; i < priv.size() - 1; i++) {
        for (int j = 0; j < priv.size() - i - 1; j++) {
            if (priv[j].get_surname() > priv[j + 1].get_surname()) {
                PhonePrivate temp = priv[j];
                priv[j] = priv[j + 1];
                priv[j + 1] = temp;
            }
        }
    }
    for (int i = 0; i < priv.size(); i++)
        priv[i].show();
    return 0; 
}
