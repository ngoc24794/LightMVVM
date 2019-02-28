# LightMVVM
Cung cấp giải pháp đơn giản thi hành mô hình MVVM

## BindableBase
Lớp cơ sở `BindableBase` thi hành giao diện `INotifyPropertyChanged` cho phép các lớp dẫn xuất có thể phát sinh sự kiện `PropertyChanged` để thông báo cho đối tượng đích `Target` của biểu thức Binding.

Trong mô hình MVVM, các `ViewModel` có thể kế thừa từ lớp `BindableBase` để thông báo cho `View` các thay đổi trên dữ liệu.
<pre><code>
    public class BindableBase : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;

        protected void Notify([CallerMemberName]string propertyName = null)
        {
            PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
        }
    }
</code></pre>

## Command
Lớp `Command` thi hành giao diện `ICommand` là khuôn mẫu để tạo lệnh.

Trong mô hình MVVM, lệnh `ICommand` là một thuộc tính của `ViewModel` cho phép thành phần `View` gọi thực thi một lệnh.

<pre><code>
    public class Command : ICommand
    {
        private readonly Action<object> _execute;
        private readonly Predicate<object> _canExecute;

        public Command(Action<object> execute, Predicate<object> canExecute = null)
        {
            _execute = execute;
            _canExecute = canExecute;
        }

        public event EventHandler CanExecuteChanged
        {
            add { CommandManager.RequerySuggested += value; }
            remove { CommandManager.RequerySuggested -= value; }
        }

        public bool CanExecute(object parameter)
        {
            return _canExecute == null || _canExecute(parameter);
        }

        public void Execute(object parameter)
        {
            _execute(parameter);
        }
    }
</code></pre>
