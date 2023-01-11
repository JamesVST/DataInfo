using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Data.OleDb;
namespace MyWindow
{
    public partial class Form1 : Form
    {
        OleDbConnection Conn;
        OleDbDataAdapter da;
        OleDbCommand cmd;
        DataSet ds;
        void GetStudent()
        {
            Conn = new OleDbConnection("Provider=Microsoft.ACE.Oledb.12.0;Data Source=dbSchool.accdb");
            da = new OleDbDataAdapter("SELECT *FROM Table1", Conn);
            ds = new DataSet();
            Conn.Open();
            da.Fill(ds, "Table1");
            dataGridView1.DataSource = ds.Tables["Table1"];
            Conn.Close();
        }


        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            GetStudent();
        }

        private void button1_Click(object sender, EventArgs e)
        {
            string query = "Insert into Table1 (FirstName,LastName) values (@fName,@lName)";
            cmd = new OleDbCommand(query, Conn);
            cmd.Parameters.AddWithValue("@fName", txtFirstName.Text);
            cmd.Parameters.AddWithValue("@lName", txtLastName.Text);
            Conn.Open();
            cmd.ExecuteNonQuery();
            Conn.Close();
            GetStudent();
        }

        private void button2_Click(object sender, EventArgs e)
        {
            string query = "Delete From Table1 Where Id=@id";
            cmd = new OleDbCommand(query, Conn);
            cmd.Parameters.AddWithValue("@id", dataGridView1.CurrentRow.Cells[0].Value);
            Conn.Open();
            cmd.ExecuteNonQuery();
            Conn.Close();
            GetStudent();

        }

        private void button3_Click(object sender, EventArgs e)
        {
            string query = "Update Table1 Set FirstName=@fName,LastName=@lName Where Id=@id";
            cmd = new OleDbCommand(query, Conn);
            cmd.Parameters.AddWithValue("@ad", txtFirstName.Text);
            cmd.Parameters.AddWithValue("@soyad", txtLastName.Text);
            cmd.Parameters.AddWithValue("@id", Convert.ToInt32(txtID.Text));
            Conn.Open();
            cmd.ExecuteNonQuery();
            Conn.Close();
            GetStudent();
        }

        private void dataGridView1_CellContentClick(object sender, DataGridViewCellEventArgs e)
        {
            txtID.Text = dataGridView1.CurrentRow.Cells[0].Value.ToString();
            txtFirstName.Text = dataGridView1.CurrentRow.Cells[1].Value.ToString();
            txtLastName.Text = dataGridView1.CurrentRow.Cells[2].Value.ToString();

        }
    }
}
# DataInfo

## Output
![adisu](https://user-images.githubusercontent.com/119830538/211708686-09e36178-3cec-401c-a21c-4e7c5db3a91d.png)
