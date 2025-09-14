using System;
using System.Collections.Generic;
using System.Linq;

namespace Lab01_02
{
    class Student
    {
        private string studentID;
        private string fullName;
        private float averageScore;
        private string faculty;

        public string StudentID { get => studentID; set => studentID = value; }
        public string FullName { get => fullName; set => fullName = value; }
        public float AverageScore { get => averageScore; set => averageScore = value; }
        public string Faculty { get => faculty; set => faculty = value; }

        public Student() { }

        public Student(string studentID, string fullName, float averageScore, string faculty)
        {
            this.studentID = studentID;
            this.fullName = fullName;
            this.averageScore = averageScore;
            this.faculty = faculty;
        }

        public void Input(List<Student> studentList)
        {
            while (true)
            {
                Console.Write("Nhap MSSV: ");
                string id = Console.ReadLine();
                if (studentList.Any(s => s.StudentID.Equals(id, StringComparison.OrdinalIgnoreCase)))
                {
                    Console.WriteLine("Loi: MSSV nay da ton tai. Vui long nhap lai.");
                }
                else
                {
                    StudentID = id;
                    break; 
                }
            }

            Console.Write("Nhap Ho Ten Sinh Vien: ");
            FullName = Console.ReadLine();

            Console.Write("Nhap Diem TB: ");
            float score;
            while (!float.TryParse(Console.ReadLine(), out score) || score < 0 || score > 10)
            {
                Console.Write("Diem khong hop le (0-10). Nhap lai: ");
            }
            AverageScore = score;

            Console.Write("Nhap Khoa: ");
            Faculty = Console.ReadLine();
        }

        public void Show()
        {
            Console.WriteLine($"MSSV: {StudentID,-10} | Ho Ten: {FullName,-25} | Khoa: {Faculty,-15} | Diem TB: {AverageScore}");
        }
    }

    internal class Program
    {
        static void Main(string[] args)
        {
            List<Student> studentList = new List<Student>();
            bool exit = false;

            while (!exit)
            {
                Console.WriteLine("======= CHUONG TRINH QUAN LY SINH VIEN =======");
                Console.WriteLine("1. Them sinh vien");
                Console.WriteLine("2. Hien thi danh sach sinh vien");
                Console.WriteLine("3. Xuat SV thuoc mot khoa bat ky");
                Console.WriteLine("4. Xuat SV co diem TB lon hon mot muc diem");
                Console.WriteLine("5. Sap xep SV theo diem TB tang dan");
                Console.WriteLine("6. Xuat SV co diem TB lon hon mot muc diem va thuoc mot khoa");
                Console.WriteLine("7. Xuat SV co diem TB cao nhat thuoc mot khoa");
                Console.WriteLine("8. Thong ke so luong tung xep loai");
                Console.WriteLine("0. Thoat");
                Console.Write("Chon chuc nang (0-8): ");
                string choice = Console.ReadLine();

                Console.Clear();

                switch (choice)
                {
                    case "1":
                        AddStudent(studentList);
                        break;
                    case "2":
                        DisplayStudentList(studentList);
                        break;
                    case "3":
                        Console.Write("Nhap ten khoa can tim: ");
                        string facultyToFind = Console.ReadLine();
                        DisplayStudentsByFaculty(studentList, facultyToFind);
                        break;
                    case "4":
                        Console.Write("Nhap muc diem toi thieu (vi du: 5): ");
                        if (float.TryParse(Console.ReadLine(), out float minScore))
                        {
                            DisplayStudentsWithHighAverageScore(studentList, minScore);
                        }
                        else
                        {
                            Console.WriteLine("Diem khong hop le.");
                        }
                        break;
                    case "5":
                        SortStudentsByAverageScore(studentList);
                        break;
                    case "6":
                        Console.Write("Nhap ten khoa can tim: ");
                        string facultyForScore = Console.ReadLine();
                        Console.Write("Nhap muc diem toi thieu: ");
                        if (float.TryParse(Console.ReadLine(), out float minScoreForFaculty))
                        {
                            DisplayStudentsByFacultyAndScore(studentList, facultyForScore, minScoreForFaculty);
                        }
                        else
                        {
                            Console.WriteLine("Diem khong hop le.");
                        }
                        break;
                    case "7":
                        Console.Write("Nhap ten khoa de tim SV co diem cao nhat: ");
                        string facultyForMax = Console.ReadLine();
                        DisplayStudentsWithHighestAverageScoreByFaculty(studentList, facultyForMax);
                        break;
                    case "8":
                        CountStudentsByGrade(studentList);
                        break;
                    case "0":
                        exit = true;
                        Console.WriteLine("Ket thuc chuong trinh. Hen gap lai!");
                        break;
                    default:
                        Console.WriteLine("Tuy chon khong hop le. Vui long chon lai.");
                        break;
                }
                
                if (!exit)
                {
                    PromptForKeyPress();
                }
            }
        }
        
        static void PromptForKeyPress()
        {
            Console.WriteLine("\nNhan phim bat ky de tiep tuc...");
            Console.ReadKey();
            Console.Clear();
        }

        static void AddStudent(List<Student> studentList)
        {
            Console.WriteLine("=== Nhap thong tin sinh vien ===");
            Student student = new Student();
            student.Input(studentList);
            studentList.Add(student);
            Console.WriteLine("Them sinh vien thanh cong!");
        }
        
        static void DisplayStudentList(List<Student> studentList, string title = "=== Danh sach sinh vien ===")
        {
            Console.WriteLine(title);
            if (!studentList.Any())
            {
                Console.WriteLine("Danh sach sinh vien trong.");
                return;
            }
            foreach (Student student in studentList)
            {
                student.Show();
            }
        }

        static void DisplayStudentsByFaculty(List<Student> studentList, string faculty)
        {
            var students = studentList.Where(s => s.Faculty.Equals(faculty, StringComparison.OrdinalIgnoreCase)).ToList();
            DisplayStudentList(students, $"=== Danh sach sinh vien thuoc khoa {faculty} ===");
        }

        static void DisplayStudentsWithHighAverageScore(List<Student> studentList, float minDTB)
        {
            var students = studentList.Where(s => s.AverageScore >= minDTB).ToList();
            DisplayStudentList(students, $"=== Danh sach SV co diem TB >= {minDTB} ===");
        }

        static void SortStudentsByAverageScore(List<Student> studentList)
        {
            var sortedStudents = studentList.OrderBy(s => s.AverageScore).ToList();
            DisplayStudentList(sortedStudents, "=== Danh sach SV sap xep theo diem TB tang dan ===");
        }

        static void DisplayStudentsByFacultyAndScore(List<Student> studentList, string faculty, float minDTB)
        {
            var students = studentList.Where(s => s.AverageScore >= minDTB
                                              && s.Faculty.Equals(faculty, StringComparison.OrdinalIgnoreCase)).ToList();
            DisplayStudentList(students, $"=== Danh sach SV co diem TB >= {minDTB} va thuoc khoa {faculty} ===");
        }

        static void DisplayStudentsWithHighestAverageScoreByFaculty(List<Student> studentList, string faculty)
        {
            var facultyStudents = studentList.Where(s => s.Faculty.Equals(faculty, StringComparison.OrdinalIgnoreCase)).ToList();
            
            if (!facultyStudents.Any())
            {
                Console.WriteLine($"Khong tim thay sinh vien nao trong khoa {faculty}.");
                return;
            }

            var maxScore = facultyStudents.Max(s => s.AverageScore);
            var students = facultyStudents.Where(s => s.AverageScore == maxScore).ToList();
            DisplayStudentList(students, $"=== SV co diem TB cao nhat ({maxScore}) thuoc khoa {faculty} ===");
        }

        static void CountStudentsByGrade(List<Student> studentList)
        {
            Console.WriteLine("=== Thong ke so luong tung xep loai ===");
            if (!studentList.Any())
            {
                Console.WriteLine("Danh sach sinh vien trong.");
                return;
            }
            
            var stats = studentList.GroupBy(s =>
            {
                if (s.AverageScore >= 9) return "Xuat sac";
                if (s.AverageScore >= 8) return "Gioi";
                if (s.AverageScore >= 7) return "Kha";
                if (s.AverageScore >= 5) return "Trung binh";
                if (s.AverageScore >= 4) return "Yeu";
                return "Kem";
            });

            foreach (var group in stats)
            {
                Console.WriteLine($"- {group.Key}: {group.Count()} sinh vien");
            }
        }
    }
}
