using MySql.Data.MySqlClient;
using System.Windows.Forms;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Linq;
using System.Text;

namespace MySQL_Library
{
    public class MySQL_eDnevnik
    {
        public static string GetMD5(string pass)
        {
            System.Security.Cryptography.MD5CryptoServiceProvider md5Provider = new System.Security.Cryptography.MD5CryptoServiceProvider();
            byte[] original = System.Text.Encoding.UTF8.GetBytes(pass);
            original = md5Provider.ComputeHash(original);
            System.Text.StringBuilder s = new System.Text.StringBuilder();
            foreach (byte b in original)
            {
                s.Append(b.ToString("x2").ToLower());
            }
            string password = s.ToString();
            return password;
        }
        public static void executeMySQL_Query(string query)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            MySqlCommand command = new MySqlCommand(query, connect);
            command.ExecuteNonQuery();
            connect.Close();
        }
        public static void readStudentsFromDatabase(ListBox theListBox)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            MySqlCommand theCommand = new MySqlCommand("SELECT firstName, lastName, clas, EGN, nomberClas, averageGrade FROM students", connect);
            MySqlDataReader reader = theCommand.ExecuteReader();
            string toShow;
            string firstName, lastName, theClass, EGN, numberClass, avgGrade;
            while (reader.Read())
            {
                for (int i = 0; i < reader.FieldCount; i++)
                {
                    firstName = reader.GetString(0);
                    lastName = reader.GetString(1);
                    theClass = reader.GetString(2);
                    EGN = reader.GetString(3);
                    numberClass = reader.GetString(4);
                    avgGrade = reader.GetString(5);
                    toShow = "Име: " + firstName + " " + lastName + " Клас: " + theClass + " ЕГН: " + EGN + " Номер в клас: " + numberClass + " Средна Оценка: " + avgGrade;
                    theListBox.Items.Add(toShow);
                }
            }
            reader.Close();
            connect.Close();
        }
        public static void addNewStudent(string FirstName, string secondName, string LastName, string TheClass, string EGN, string NumberInClass)
        {
            string theCommand = "INSERT INTO students(EGN, firstName, lastName, clas, nomberClas, secondName) VALUES(" + EGN + ", '" + FirstName + "', '" + LastName + "', " + TheClass + ", " + NumberInClass + ", '"+secondName+"')";
            MySQL_eDnevnik.executeMySQL_Query(theCommand);
        }
        public static void addNewTeacher(string FirstName, string LastName, string subject, string pass, string email)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            MySqlCommand getSubjectID = new MySqlCommand("SELECT ID FROM subjects WHERE subject LIKE '" + subject + "'", connect);
            MySqlDataReader subjectReader = getSubjectID.ExecuteReader();
            string subjectId="";
            while(subjectReader.Read())
            {
                subjectId = subjectReader.GetString(0);
            }
            subjectReader.Close();
            string halfPass = GetMD5(pass);
            string securedPass = GetMD5(halfPass + "limonada");
            string theCommand = "INSERT INTO teachers(FirstName, LastName, predmet, pass, email) VALUES('" + FirstName + "', '" + LastName + "', " + subjectId + ", '" + securedPass + "', '" + email + "')";
            MySqlCommand command = new MySqlCommand(theCommand, connect);
            command.ExecuteNonQuery();
            connect.Close();
        }
        public static void addNewSubject(string subject)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            string command = "INSERT INTO subjects(subject) VALUES('" + subject + "')";
            MySqlCommand theCommand = new MySqlCommand(command, connect);
            theCommand.ExecuteNonQuery();
            connect.Close();
        }
        public static void addNewGrade(string Teacher, string Student, string Subject, string Grade)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            string getTeacher = "SELECT ID FROM teachers WHERE email LIKE '"+Teacher+"'";
            MySqlCommand getTeacherCommand = new MySqlCommand(getTeacher, connect);
            MySqlDataReader readTeacherId = getTeacherCommand.ExecuteReader();
            string teacherId = "";
            while (readTeacherId.Read())
            {
                teacherId = readTeacherId.GetString(0);
            }
            readTeacherId.Close();
            string getSubjectID = "SELECT ID FROM subjects WHERE subject LIKE '" + Subject+"'";
            MySqlCommand readSubjectId = new MySqlCommand(getSubjectID, connect);
            MySqlDataReader subjectReader = readSubjectId.ExecuteReader();
            string subjectId = "";
            while (subjectReader.Read())
            {
                subjectId = subjectReader.GetString(0);
            }
            subjectReader.Close();
            DateTime now = DateTime.Now;
            string year = now.Year.ToString();
            string month = now.Month.ToString();
            string day = now.Day.ToString();
            string currentDate = year + "-" + month + "-" + day;
            string addGrade = "INSERT INTO grades(teacherID, studentID, subjectID, grade, date) VALUES(" + teacherId + ", " + Student + ", " + subjectId + ", " + Grade + ", '"+currentDate+"')";
            MySqlCommand addGradeCommand = new MySqlCommand(addGrade, connect);
            addGradeCommand.ExecuteNonQuery();
            connect.Close();           

        }
        public static void deleteGrade(string gredeID)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            string delete = "DELETE FROM grades WHERE ID = "+gredeID;
            MySqlCommand deleteCommand = new MySqlCommand(delete, connect);
            deleteCommand.ExecuteNonQuery();
            connect.Close();
        }
        public static void editGrade(string gradeID, string newGrade)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            string update = "UPDATE grades SET grade=" + newGrade + " WHERE ID=" + gradeID;
            MySqlCommand updateCommand = new MySqlCommand(update, connect);
            updateCommand.ExecuteNonQuery();
            connect.Close();
        }
        public static void editGradeStudent(string gradeID, string newStudent)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            string update = "UPDATE grades SET studentID=" + newStudent + " WHERE ID=" + gradeID;
            MySqlCommand updateCommand = new MySqlCommand(update, connect);
            updateCommand.ExecuteNonQuery();
            connect.Close();
        }
        public static void addNewMissing(string studentID, string subject)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            string getSubjectID = "SELECT ID FROM subjects WHERE subject LIKE '" + subject + "'";
            MySqlCommand readSubjectId = new MySqlCommand(getSubjectID, connect);
            MySqlDataReader subjectReader = readSubjectId.ExecuteReader();
            string subjectId = "";
            while (subjectReader.Read())
            {
                subjectId = subjectReader.GetString(0);
            }
            subjectReader.Close();
            DateTime now = DateTime.Now;
            string year = now.Year.ToString();
            string month = now.Month.ToString();
            string day = now.Day.ToString();
            string currentDate = year + "-" + month + "-" + day;
            string addMissing = "INSERT INTO  misings(studentID, subjectID, date) VALUES(" + studentID + ", " + subjectId + ", '"+currentDate+"')";
            MySqlCommand addMissingCommand = new MySqlCommand(addMissing, connect);
            addMissingCommand.ExecuteNonQuery();
            connect.Close();
        }
        public static void deleteMissing(string missingId)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            string delete = "DELETE FROM misings WHERE ID=" + missingId;
            MySqlCommand deleteCommand = new MySqlCommand(delete, connect);
            deleteCommand.ExecuteNonQuery();
            connect.Close();
        }
        public static void deleteStudent(string studentId)
        {
            string delete = "DELETE FROM students WHERE ID=" + studentId;
            MySQL_eDnevnik.executeMySQL_Query(delete);
        }
        public static void editStudent(string studentId, string newFirstName, string newSecondName, string newLastName, string newNumber, string newClass)
        {
            string edit = "UPDATE students SET firstName='" + newFirstName + "', secondName='" + newSecondName + "', lastName='" + newLastName + "', clas='" + newClass + "', nomberClas='" + newNumber + "' WHERE ID=" + studentId;
            MySQL_eDnevnik.executeMySQL_Query(edit);
        }
        public static void deleteSubject(string subject)
        {
            string delete = "DELETE FROM subjects WHERE subject LIKE '" + subject + "'";
            MySQL_eDnevnik.executeMySQL_Query(delete);
        }
        public static void deleteTeacher(string teacher)
        {
            string delete = "DELETE FROM teachers WHERE email LIKE '" + teacher + "'";
            MySQL_eDnevnik.executeMySQL_Query(delete);
        }
        public static void editTeacher(string email, string firstName, string lastName, string subject)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            string getSubjectID = "SELECT ID FROM subjects WHERE subject LIKE '" + subject + "'";
            MySqlCommand readSubjectId = new MySqlCommand(getSubjectID, connect);
            MySqlDataReader subjectReader = readSubjectId.ExecuteReader();
            string subjectId = "";
            while (subjectReader.Read())
            {
                subjectId = subjectReader.GetString(0);
            }
            subjectReader.Close();
            string edit = "UPDATE teachers SET firstName='"+firstName+"', lastName='"+lastName+"', predmet="+subjectId+" WHERE email LIKE '"+email+"'";
            MySQL_eDnevnik.executeMySQL_Query(edit);
            connect.Close();
        }
        public static void addNewWarning(string teacher, string student, string text)
        {
            MySqlConnection connect = new MySqlConnection("SERVER=localhost; UID=root; DATABASE=sapmenet_ednevnik; PASSWORD=");
            connect.Open();
            string getTeacher = "SELECT ID FROM teachers WHERE email LIKE '" + teacher + "'";
            MySqlCommand getTeacherCommand = new MySqlCommand(getTeacher, connect);
            MySqlDataReader readTeacherId = getTeacherCommand.ExecuteReader();
            string teacherId = "";
            while (readTeacherId.Read())
            {
                teacherId = readTeacherId.GetString(0);
            }
            string add = "INSERT INTO warmings(studentID, teacherID, text) VALUES(" + student + ", " + teacherId + ", '" + text + "')";
            MySQL_eDnevnik.executeMySQL_Query(add);
        }
        public static void deleteWarning(string warning)
        {
            string delete = "DELETE FROM warmings WHERE ID=" + warning;
            MySQL_eDnevnik.executeMySQL_Query(delete);
        }
        
    }
}
