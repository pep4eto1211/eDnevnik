using System;
using System.Windows.Forms;
using MySQL_Library;
using System.Text;

namespace MySQL_Test
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }
        private void button1_Click(object sender, EventArgs e)
        {
            MySQL_eDnevnik.addNewStudent("Sashomir", "Sashomirov", "sashov", "10", "6543213242", "13");
        }

        private void listBox1_SelectedIndexChanged(object sender, EventArgs e)
        {
            string selected = listBox1.SelectedItem.ToString();
            textBox1.Text = selected;
        }
    }
}
