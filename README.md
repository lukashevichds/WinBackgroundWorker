Лабораторная работа 7. Асинхронное программирование
Цель работы
Изучение возможностей, реализующих асинхронное
программирование и получение навыков по работе в программе с потоками.
Упражнение 1. Работа с компонентом BackgroundWorker
1.	Создайте новый проект Windows Forms. Назовите его WinBackgroundWorker.
![image](https://github.com/user-attachments/assets/d6f97526-9003-41c3-92ba-841fd3b363b4)
2. Добавьте на форму элементы управления: два элемента Label,
TextBox, ProgressBar и две кнопки Button.
![image](https://github.com/user-attachments/assets/1f4f1131-77bd-4993-9b0c-cd424e21c4ca)
3. Для элементов укажите свойства в соответствии с таблицей:
   ![image](https://github.com/user-attachments/assets/4ae740ce-4f5a-4b4e-b0a5-7465b35bf330)
![image](https://github.com/user-attachments/assets/38254e28-aa5a-4ad0-ab1b-983374225313)
все указал
4. Для элемента textBox1 допустимыми значениями будут только
цифры, поэтому в обработчике события KeyPress для текстового поля
textBox1 укажите код:
if (!char.IsDigit(e.KeyChar))
{
e.Handled = true;
MessageBox.Show("Поле должно содержать цифры");
}
![image](https://github.com/user-attachments/assets/4a023ce1-49ee-4920-8d9a-b684b3b79c46)
5. Из Toolbox перетащите элемент BackgroundWorker в форму.
   ![image](https://github.com/user-attachments/assets/0b546b59-a485-45ab-89d2-4c2193f472cc)
Перетащил
6. В окне Properties установите свойства WorkerSupportsCancellation
и WorkerReportsProgress в True (для поддержки асинхронной отмены и
возможности сообщения основному потоку информации о продвижении
фонового процесса соответственно).
![image](https://github.com/user-attachments/assets/cd08a000-34aa-4984-a230-90d8ab035546)
7. Дважды щелкните BackgroundWorker, чтобы открыть обработчик
события backgroundWorker1_DoWork по умолчанию. Добавьте к этому
обработчику события следующий код:
int i;
i = int.Parse(e.Argument.ToString());
for (int j=1; j <= i; j++)
{
if (backgroundWorker1.CancellationPending)

{
e.Cancel = true;
return;
}
System.Threading.Thread.Sleep(1000);
backgroundWorker1.ReportProgress((int)(j *100/ i));
}
![image](https://github.com/user-attachments/assets/9a2ef191-e6bc-4477-ab5d-c11bb05aafd9)
8. Для элемента backgroundWorkerl в окне Properties щелкните
кнопку Events, затем дважды щелкните ProgressChanged, чтобы открыть
окно кода обработчика события backgroundWorker1_ProgressChanged.
Добавьте следующий код:
progressBar1.Value = e.ProgressPercentage;
![image](https://github.com/user-attachments/assets/e49a30d7-7fd1-48f2-b6a7-8004565557d0)
![image](https://github.com/user-attachments/assets/f26ecbf0-49df-4d44-be22-9fda8f005b70)
9. Аналогичным способом добавьте обработчик события
RunWorkerCompleted, в теле обработчика добавьте следующий код:
if (!(e.Cancelled))
System.Windows.Forms.MessageBox.Show("Run Completed!");
else
System.Windows.Forms.MessageBox.Show("Run Cancelled");
![image](https://github.com/user-attachments/assets/5f1832d0-f486-4d06-b818-74cc9560bce5)
![image](https://github.com/user-attachments/assets/e4f3ef63-78ad-4a2c-bc50-33ddac453bf3)
10. Для кнопки Start откройте обработчик события Click. Добавьте
следующий код:
if (!(textBox1.Text == ""))
{
int i = int.Parse(textBox1.Text);
backgroundWorker1.RunWorkerAsync(i);
}
![image](https://github.com/user-attachments/assets/71d69ebc-c4fd-4eca-be21-e865774f2848)
11. Для кнопки Cancel откройте обработчик события Click. Добавьте
следующий код
backgroundWorker1.CancelAsync();
![image](https://github.com/user-attachments/assets/22242c74-ddde-4cf9-8ac9-846dd080e85b)
12. Построите и выполните приложение, и проверьте его
функциональность.
Ничего не получилось
![image](https://github.com/user-attachments/assets/16e3b70f-1cd4-4c61-a327-502b016910b9)
Вроде бы сделал все по заданию, но пишет, что у меня ошибка, не могу понять, как ее решить
![image](https://github.com/user-attachments/assets/2ff56a34-4b3e-4e5e-a927-1e7e4b24c464)
Упражнение 2. Использование делегатов
1.	Создайте новое Windows-приложение и назовите его WinAsynchDelegate.
![image](https://github.com/user-attachments/assets/0771e6c9-81c4-4ebb-8d29-4ea5830438d4)
2. Повторите шаги 2 – 4 предыдущего упражнения для создания
требуемой формы.
![image](https://github.com/user-attachments/assets/b3d425ae-9af1-42ff-95af-f180aeff890b)
Повторил
3. Добавьте в приложение следующий метод:
private void TimeConsumingMethod(int seconds)
{
for (int j = 1; j <= seconds; j++)
System.Threading.Thread.Sleep(1000);
}
![image](https://github.com/user-attachments/assets/64487dab-1d1c-4419-b1c0-86c9f0f1c67e)
4. Используйте в приложении делегат, соответствующий методу
TimeConsumingMethod:
private delegate void TimeConsumingMethodDelegate(int
seconds);
![image](https://github.com/user-attachments/assets/95c35910-11d5-4703-ba72-264096b294fe)
5. Добавьте сообщающий о продвижении операции метод, который
устанавливает значение элемента управления ProgressBar
потокобезопасным способом, и делегата этого метода:
public delegate void SetProgressDelegate(int val);
public void SetProgress(int val)
{
if (progressBar1.InvokeRequired)
{
SetProgressDelegate del = new
SetProgressDelegate(SetProgress);
this.Invoke(del, new object[] { val });
}
else
{
progressBar1.Value = val;
}
}
![image](https://github.com/user-attachments/assets/5fbf8d5a-6e95-47ff-b763-d9516fff8bc1)
6. Для сообщения о продвижении операции добавьте следующую
строку кода в цикл For метода TimeConsumingMethod:
SetProgress((int)(j * 100) / seconds);
![image](https://github.com/user-attachments/assets/c4c82a71-553a-4fdc-a656-cfde908dd0ff)
Странно, j существует, стоит в цикле for…
![image](https://github.com/user-attachments/assets/4cc617a0-fc89-4ed9-820b-f79a4088ca05)
Если поставить 2 for, то вроде нормально
7. Добавьте в приложение логическую переменную с именем Cancel:
bool Cancel;
![image](https://github.com/user-attachments/assets/d40b17b6-4628-4524-b895-10518b616655)
8. Добавьте следующие строки кода в цикл For метода
TimeConsumingMethod:
if (Cancel)
break;
![image](https://github.com/user-attachments/assets/78d5531c-2e0c-48bd-87ac-fb376da856a9)
9. Добавьте следующий код после цикла For метода
TimeConsumingMethod:
if (Cancel)
{
System.Windows.Forms.MessageBox.Show("Cancelled");
Cancel = false;
}
else
{
System.Windows.Forms.MessageBox.Show("Complete");
}
![image](https://github.com/user-attachments/assets/0cc55a0c-3bc2-4f27-bea6-24a09f70ee2b)
10. В конструкторе дважды щелкните кнопку GO!, чтобы открыть для
нее обработчик события Click по умолчанию, и добавьте следующий код:
TimeConsumingMethodDelegate del = new
TimeConsumingMethodDelegate(TimeConsumingMethod);
del.BeginInvoke(int.Parse(textBox1.Text), null, null);
Может быть кнопку “Start”?
![image](https://github.com/user-attachments/assets/65137089-d32c-4b9b-a4b7-96fcf349efd0)
11. В конструкторе дважды щелкните кнопку Cancel, чтобы открыть
для нее обработчик события Click по умолчанию, и добавьте следующий
код:
Cancel = true;
![image](https://github.com/user-attachments/assets/cc32dbed-8eba-4345-963c-bc7676dc08a6)
Странно
![image](https://github.com/user-attachments/assets/08422597-f902-40b1-9f56-d91b83f1a2b3)
Так-то лучше
12. Скомпилируйте и проверьте ваше приложение
    ![image](https://github.com/user-attachments/assets/fc516be6-acdf-49fc-9663-0c077ca0a4ba)
Не получилось из-за ошибок, как их устранить, я не знаю…

Упражнение 3. Асинхронный запуск произвольного метода
1. Создайте новое Windows-приложение и назовите его
WinAsynchMethod.
![image](https://github.com/user-attachments/assets/69505922-1268-4daa-aa0e-aaafe6ce8303)
2. Установите свойствам формы Size значение 425;200 и Text –
"Асинхронный запуск".
![image](https://github.com/user-attachments/assets/27c118d1-ccf5-48c7-9ce2-6b3d30f2d665)
3. Добавьте на форму три надписи, два текстовых поля и две кнопки, и
установите им следующие свойства:
![image](https://github.com/user-attachments/assets/dc6e2843-2c48-4316-b6f9-eb1deb559899)
![image](https://github.com/user-attachments/assets/cbb68f7f-0a54-4c0d-9c02-e65bd71e91c9)
4. Создайте делегат:
private delegate int AsyncSumm(int a, int b);
![image](https://github.com/user-attachments/assets/b51cd163-380c-4317-a315-fee03e90db81)
5. Создайте метод Summ, в котором будут складываться числа,
вводимые в два текстовых поля, и укажите задержку операции на 9 секунд:
private int Summ(int a, int b)
{
System.Threading.Thread.Sleep(9000);
return a+b;
}
![image](https://github.com/user-attachments/assets/7f703179-bde9-4dab-b572-9fa52bdfabcc)
6. Реализуйте обработчик кнопки btnRun, который будет включать
также действия по организации асинхронного вызова:
Создайте экземпляр делегата и проинициализируйте его методом
Summ:
AsyncSumm summdelegate = new AsyncSumm(Summ);
Конечный итог:
![image](https://github.com/user-attachments/assets/ade50e2d-0d03-4e74-9c8d-9c0d661f9124)
8. Создайте метод CallBackMethod, который привязан к делегату
summdelegate:
private void CallBackMethod(IAsyncResult ar)
{
string str;
AsyncSumm summdelegate = (AsyncSumm)ar.AsyncState;
str = String.Format("Сумма введенных чисел равна
{0}", summdelegate.EndInvoke(ar));
MessageBox.Show(str, "Результат операции");
}
![image](https://github.com/user-attachments/assets/7fbe6be1-9b54-47e0-b61d-3c62e7ada4fb)
Конечный итог:
![image](https://github.com/user-attachments/assets/5fb6e31a-66b0-446d-80f9-85842ed17450)
9. Для демонстрации асинхронности выполнения метода реализуйте
обработчик нажатия кнопки Работа, например, следующим образом:
MessageBox.Show("Работа кипит!!!");
![image](https://github.com/user-attachments/assets/437665b6-9e22-42ea-bf8b-578d035f8b52)
10. Постройте и запустите приложение. После нажатия кнопки
Сумма, пока будет выполняться операция, нажмите кнопку Работа.
Проверьте, что метод суммирования действительно реализован
асинхронно.
![image](https://github.com/user-attachments/assets/adb6e6aa-789a-46eb-a455-2a8caf153de9)
![image](https://github.com/user-attachments/assets/a7703488-109d-4d2a-beef-605dff13b875)
нажал, и ничего не произошло
