# Folder1

using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Windows.Forms;
using System.IO.Ports;

namespace WindowsFormsApplication1
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        private void label1_Click(object sender, EventArgs e)
        {

        }

        private void button1_Click(object sender, EventArgs e)
        {
            serialPort1.PortName = comboBox1.Text;
            if (!serialPort1.IsOpen)
            {
                serialPort1.Open();
                button1.Enabled = false;
                button2.Enabled = true;
            }
            else
            {
                MessageBox.Show("Failed To Open Port");
            }
        }

        private void button2_Click(object sender, EventArgs e)
        {
            if (serialPort1.IsOpen)
            {
                serialPort1.Close();
                button1.Enabled = true;
                button2.Enabled = false;
            }

        }

        private void serialPort1_DataReceived(object sender, System.IO.Ports.SerialDataReceivedEventArgs e)
        {
            string dataSerial;
            dataSerial = serialPort1.ReadExisting();
            updateText(dataSerial);
        }
        delegate void updateTextFromDelegate(String msg);
        public void updateText(String msg)
        {
            if (!this.IsDisposed && richTextBox1.InvokeRequired)
            {
                richTextBox1.Invoke(new
updateTextFromDelegate(updateRichTextBox), new Object[] { msg });
            }
        }
        public void updateRichTextBox(String msg)
        {
            richTextBox2.Text = msg;
            richTextBox2.SelectionStart =
richTextBox2.Text.Length;
        }

        private void button3_Click(object sender, EventArgs e)
        {
            serialPort1.Write(richTextBox1.Text);
            richTextBox2.Text = "";
        }
        


        private void Form1_Load_1(object sender, EventArgs e)
        {
            foreach (string item in SerialPort.GetPortNames())
            {
                button1.Enabled = true;
                button2.Enabled = false;
                comboBox1.Items.Add(item);
                comboBox1.SelectedIndex = 0;
            }
        }
    }
}


