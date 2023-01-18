# C# 使用程序標準輸入與輸出

在使用 cmd 呼叫某一支程式起來執行時，有時該程式有參數需要帶入才能做後續的事情，ex.  mysql.exe 的程式，通常較這隻程式之後，他會在叫你輸入資料庫的密碼，但是，假設今天需要讓輸入密碼的這個動作也透過程式自行完成，為此，本篇的主要在說明 C# 中的 Process 類別，將 Shell 的控制權，全部由程式來操作。

主要就是要記得把 UseShellExectute 變成 false，不由 windows shell 執行，這樣 RedirectStandardInput 與 RedirectStandardOut 設定為 true 才能改由我們去控制輸出入的標準流。

這邊使用 StreamWriter 作為標準輸入的介面，最後在標準輸出的地方，使用 Process 的 OutputDataReceived 事件註冊，當有任何輸出都可以被接收到，並且要記得開始接收輸出，BeingOutputReadLine()，如此，即可都由程式自行控制輸入輸出流了。

一個簡單的概念是，自行控制標準輸入與輸出，以下是範例程式碼：

```cs
namespace ProcesStandardInputTest
{
    class Program
    {
        static void Main(string[] args)
        {
            using (Process process = new Process())
            {
                process.StartInfo.FileName = "cmd.exe";
                process.StartInfo.UseShellExecute = false;
                process.StartInfo.RedirectStandardInput = true;
                process.StartInfo.RedirectStandardOutput = true;
                process.StartInfo.CreateNoWindow = false;
                process.Start();

                StreamWriter writer = process.StandardInput;

                string inputText;
                int numLines = 0;

                process.OutputDataReceived += OutputDataReceived;

                process.BeginOutputReadLine();

                do {
                    inputText = Console.ReadLine();

                    if(inputText.Length > 0)
                    {
                        numLines++;
                        writer.WriteLine(inputText);
                    }
                } while(inputText.Length > 0);

                // Write a report header to the console.
                if (numLines > 0)
                {
                    Console.WriteLine($"{numLines} sorted text line(s)");
                    Console.WriteLine("---------------------------------");
                }
                else
                {
                    Console.WriteLine("No input was sorted");
                }

                // End the input stream to the sort command.
                // When the stream closes, the sort command
                // writes the sorted text lines to the
                // console.
                writer.Close();

                // Wait for the sort process to write the sorted text lines.
                process.WaitForExit();
            }

            Console.WriteLine("Hello World!");
            System.Console.ReadLine();
        }

        private static void OutputDataReceived(object sender, DataReceivedEventArgs e)
        {
            // System.Console.WriteLine(\"start\");

            Console.WriteLine(e.Data);

            // System.Console.WriteLine(\"end\");
        }
    }
}
```