# Код приложения
## Код программы 

```C#
public partial class MainForm : Form
    {
        string file_path = "";
        Form InfoForm = new Form();
        public MainForm()
        {
            InitializeComponent();
            InterfaceOption interfaceOption = new InterfaceOption();
            RegistryKey txtRedOption = Registry.CurrentUser;
            RegistryKey Interface = txtRedOption.CreateSubKey("Interface");
            Color mc = Color.FromName(Interface.GetValue("MC").ToString());
            Color mpc = Color.FromName(Interface.GetValue("MPC").ToString());
            Color cc = Color.FromName(Interface.GetValue("CC").ToString());
            Color tc = Color.FromName(Interface.GetValue("TC").ToString());
            try
            {
                BackColor = mc;

            }
            catch
            {
                BackColor = Color.Gray;
                foreach (ToolStripComboBox micb in toolStrip1.Items.OfType<ToolStripComboBox>())
                {
                    micb.BackColor = Color.Gray;
                    micb.ForeColor = Color.Black;
                }
            }
            finally
            {
                ForeColor = tc;
                menuStrip1.BackColor = cc;
                menuStrip1.ForeColor = tc;
                statusStrip1.BackColor = cc;
                statusStrip1.ForeColor = tc;
                toolStrip1.BackColor = mpc;
                toolStrip1.ForeColor = tc;
                foreach (ToolStripMenuItem mi in menuStrip1.Items.OfType<ToolStripMenuItem>())
                {
                    mi.BackColor = cc;
                    mi.ForeColor = tc;
                    foreach (ToolStripItem ddi in mi.DropDownItems.OfType<ToolStripItem>())
                    {
                        ddi.BackColor = cc;
                        ddi.ForeColor = tc;
                    }
                    foreach (ToolStripSeparator ssi in mi.DropDownItems.OfType<ToolStripSeparator>())
                    {
                        ssi.BackColor = cc;
                        ssi.ForeColor = tc;
                    }
                }
            }

        }

        private void ToolStripMenuItem2_Click(object sender, EventArgs e)
        {

        }

        private void НовыйToolStripMenuItem_Click(object sender, EventArgs e)
        {
            Readactor readactor = new Readactor();
            readactor.Form_Create("New_file", this);
        }

        private void ОткрытьToolStripMenuItem_Click(object sender, EventArgs e)
        {
            openFileDialog1.ShowDialog();
        }

        private void СохранитьToolStripMenuItem_Click(object sender, EventArgs e)
        {
            switch (file_path != "")
            {
                case (true):
                    if (File.Exists(file_path))
                    {
                        FileInfo fileInfo = new FileInfo(file_path);
                        switch (fileInfo.Extension)
                        {
                            case ("txt"):
                                StreamWriter writer = new StreamWriter(file_path);
                                writer.Write(Program.RedactTB.Text);
                                writer.Close();
                                break;
                            case ("doc"):
                                word.Application MSOW97 = new word.Application();
                                word.Document document97 = MSOW97.Documents.Add(Visible: true);
                                word.Paragraph paragraph97 = document97.Paragraphs.Add();
                                paragraph97.Range.Text = Program.RedactTB.Text;
                                paragraph97.Range.Font.Name = Program.Font_Name;
                                paragraph97.Range.Font.Size = Program.Font_Size;
                                document97.SaveAs2(file_path, word.WdSaveFormat.wdFormatDocument97);
                                document97.Close();
                                MSOW97.Quit();
                                break;
                            case ("docx"):
                                word.Application MSOW = new word.Application();
                                word.Document document = MSOW.Documents.Add(Visible: true);
                                word.Paragraph paragraph = document.Paragraphs.Add();
                                paragraph.Range.Text = Program.RedactTB.Text;
                                paragraph.Range.Font.Name = Program.Font_Name;
                                paragraph.Range.Font.Size = Program.Font_Size;
                                document.SaveAs2(file_path, word.WdSaveFormat.wdFormatDocumentDefault);
                                document.Close();
                                MSOW.Quit();
                                break;
                        }
                    }
                    else
                    {
                        Readactor readactor = new Readactor();
                        readactor.save_dialog_execute();
                    }
                    break;
                case (false):

                    break;
            }

        }

        private void СохранитьКакToolStripMenuItem_Click(object sender, EventArgs e)
        {
            Readactor readactor = new Readactor();
            readactor.save_dialog_execute();

        }

        private void ЗакрытьФайлToolStripMenuItem_Click(object sender, EventArgs e)
        {
            Form form = ActiveMdiChild;
            form.Close();
        }

        private void ВыходToolStripMenuItem_Click(object sender, EventArgs e)
        {
            this.Close();
        }

        private void НастройкиИнтерфейсаToolStripMenuItem_Click(object sender, EventArgs e)
        {
            InterfaceOption interfaceOption = new InterfaceOption();
            interfaceOption.Show(this);
        }

        private void СтатистикаСимволовToolStripMenuItem_Click(object sender, EventArgs e)
        {
            ChartStaticstic chart = new ChartStaticstic();
            chart.Show();
        }

        private void ОПрограммеToolStripMenuItem_Click(object sender, EventArgs e)
        {
            InfoForm.Height = 175;
            InfoForm.Width = 250;
            InfoForm.FormBorderStyle = FormBorderStyle.None;
            InfoForm.StartPosition = FormStartPosition.CenterScreen;
            InfoForm.BackColor = Color.Black;
            Panel panel = new Panel();
            panel.Dock = DockStyle.Bottom;
            panel.Height = 25;
            InfoForm.Controls.Add(panel);
            Button button = new Button();
            Label label = new Label();
            label.Dock = DockStyle.Fill;
            label.ForeColor = Color.White;
            label.Text = "Текстовый реадктор. \n\n\nВ возможность входит: работа с файлами форматов (txt, Microsoft Word, Microsoft Exel, PDF). Построение диаграмм символов. \n\n\n\n Разработчик: Лебедева Анастасия";
            button.FlatStyle = FlatStyle.Flat;
            button.ForeColor = Color.White;
            button.Text = "Закрыть";
            button.Click += button_click;
            panel.Controls.Add(button);
            InfoForm.Controls.Add(label);
            InfoForm.ShowDialog();

        }

        private void button_click(object sender, EventArgs e)
        {
            InfoForm.Close();
        }


        private void ToolStripButton1_Click(object sender, EventArgs e)
        {
            switch (toolStripButton1.Pressed)
            {
                case (true):
                    Program.style = Program.style ^ FontStyle.Bold;
                    break;
                case (false):
                    Program.style = Program.style | FontStyle.Bold;
                    break;
            }
            Program.RedactTB.SelectionFont = new Font(Program.Font_Name, Program.Font_Size, Program.style);

        }

        private void ToolStripButton2_Click(object sender, EventArgs e)
        {
            switch (toolStripButton2.Pressed)
            {
                case (true):
                    Program.style = Program.style ^ FontStyle.Italic;
                    break;
                case (false):
                    Program.style = Program.style | FontStyle.Italic;
                    break;
            }
            Program.RedactTB.SelectionFont = new Font(Program.Font_Name, Program.Font_Size, Program.style);

        }

        private void ToolStripButton3_Click(object sender, EventArgs e)
        {
            switch (toolStripButton3.Pressed)
            {
                case (true):
                    Program.style = Program.style ^ FontStyle.Underline;
                    break;
                case (false):
                    Program.style = Program.style | FontStyle.Underline;
                    break;
            }
            Program.RedactTB.SelectionFont = new Font(Program.Font_Name, Program.Font_Size, Program.style);

        }

        private void ToolStripButton4_Click(object sender, EventArgs e)
        {
            Program.RedactTB.SelectionAlignment = HorizontalAlignment.Left;
        }

        private void ToolStrip1_ItemClicked(object sender, ToolStripItemClickedEventArgs e)
        {

        }

        private void ToolStripButton5_Click(object sender, EventArgs e)
        {
            Program.RedactTB.SelectionAlignment = HorizontalAlignment.Center;
        }

        private void ToolStripButton6_Click(object sender, EventArgs e)
        {
            Program.RedactTB.SelectionAlignment = HorizontalAlignment.Right;
        }

        private void ToolStripButton7_Click(object sender, EventArgs e)
        {

        }

        private void ToolStripButton8_Click(object sender, EventArgs e)
        {

        }

        private void ToolStripButton9_Click(object sender, EventArgs e)
        {

        }

        private void ToolStripComboBox1_SelectedIndexChanged(object sender, EventArgs e)
        {
            Program.Font_Name = toolStripComboBox1.Text;
            if (Program.RedactTB != null)
            {
                Program.RedactTB.SelectionFont = new Font(Program.Font_Name, Program.Font_Size, Program.style);
            }
        }

        private void ToolStripComboBox2_SelectedIndexChanged(object sender, EventArgs e)
        {
            Program.Font_Size = Convert.ToSingle(toolStripComboBox2.Text);
            if (Program.RedactTB != null)
            {
                Program.RedactTB.SelectionFont = new Font(Program.Font_Name, Program.Font_Size, Program.style);
            }
        }

        private void Timer1_Tick(object sender, EventArgs e)
        {
            toolStripStatusLabel1.Text = DateTime.Now.ToLongDateString() + " " + DateTime.Now.ToLongTimeString();
        }

        private void OpenFileDialog1_FileOk(object sender, CancelEventArgs e)
        {
            if (openFileDialog1.FileName != "")
            {
                file_path = openFileDialog1.FileName;
                Readactor readactor = new Readactor();
                readactor.Form_Create(openFileDialog1.FileName, this);
                switch (openFileDialog1.FilterIndex)
                {
                    case (1):
                        if (File.Exists(openFileDialog1.FileName))
                        {
                            StreamReader reader = new StreamReader(openFileDialog1.FileName);
                            Program.RedactTB.Text = reader.ReadToEnd();
                            reader.Close();
                        }
                        break;
                    case (2):
                        word.Application application = new word.Application();
                        word.Document documents = application.Documents.Open(openFileDialog1.FileName);
                        try
                        {
                            for (int i = 0; i < documents.Paragraphs.Count; ++i)
                            {
                                Program.RedactTB.Font = new Font(documents.Paragraphs[i + 1].Range.Font.Name, documents.Paragraphs[i + 1].Range.Font.Size);
                                Program.RedactTB.AppendText(documents.Paragraphs[i + 1].Range.Text.ToString());
                            }
                        }
                        catch
                        {

                        }
                        finally
                        {
                            documents.Close();
                            application.Quit();
                        }
                        break;
                    case (3):
                        word.Application application1 = new word.Application();
                        word.Document documents1 = application1.Documents.Open(openFileDialog1.FileName);
                        try
                        {
                            for (int i = 0; i < documents1.Paragraphs.Count; ++i)
                            {
                                Program.RedactTB.Font = new Font(documents1.Paragraphs[i + 1].Range.Font.Name, documents1.Paragraphs[i + 1].Range.Font.Size);
                                Program.RedactTB.AppendText(documents1.Paragraphs[i + 1].Range.Text.ToString());
                            }
                        }
                        catch
                        {

                        }
                        finally
                        {
                            documents1.Close();
                            application1.Quit();
                        }
                        break;
                }
            }
            else
            {
                MessageBox.Show("Выберите файл", "Текстовый реадктор", MessageBoxButtons.OK, MessageBoxIcon.Warning);
            }

        }

        private void MainForm_Load(object sender, EventArgs e)
        {
            toolStripComboBox2.SelectedIndex = 0;
            foreach (FontFamily font in FontFamily.Families)
            {
                toolStripComboBox1.Items.Add(font.Name);
            }
            toolStripComboBox1.SelectedIndex = toolStripComboBox1.FindStringExact("Arial");
            Program.Font_Name = toolStripComboBox1.Text;
            Program.Font_Size = Convert.ToSingle(toolStripComboBox2.Text);

        }

        private void MainForm_FormClosing(object sender, FormClosingEventArgs e)
        {
            switch (MessageBox.Show("Зaкрыть программу?", "Текстовый редактор", MessageBoxButtons.YesNo, MessageBoxIcon.Question))
            {
                case (DialogResult.Yes):
                    e.Cancel = false;
                    break;
                case (DialogResult.No):
                    e.Cancel = true;
                    break;
            }
        }
    }
```

(c) Ксения, 2020 г. 
