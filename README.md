# C1Logger

### My Website: [Corpi1.uk](https://Corpi1.uk)

C1Log is a modern, thread-safe, and highly configurable logging library for .NET applications.  
It supports asynchronous logging, file rolling (by date and size), minimum log level filtering, syslog-style log levels (with shorthand), and custom log sinks for advanced scenarios.

---

## Installation

To use this library, install it via NuGet in your project. You can do this by running the following command in the Package Manager Console:

Latest Version:

```bash
NuGet\Install-Package C1.Logger
```

Specific Version:
```bash
NuGet\Install-Package C1.Logger --Version x.x.x
```


Alternatively, search for "C1Log" in the NuGet Package Manager in Visual Studio and install it from there.

---

## Usage

For a full usage guide, please refer to the [Docs](https://docs.corpi1.uk/C1Logger/Introduction)

> **Note:**  
> C1Log is fully self-initializing. You do **not** need to call any initialization method.  
> If you want to customize configuration (log directory, minimum log level, formatter, etc.), set those properties **before** your first log call.

**Log Levels:**
- Emergency (`emerg`)
- Alert (`alert`)
- Critical (`crit`)
- Error (`error`)
- Warning (`warn`)
- Notice (`notice`)
- Informational (`info`)
- Debug (`debug`)

**Basic Examples:**

Default usage with default settings:
```csharp
using C1Logger; 
C1Log.Info("Application started."); 
C1Log.Flush(); // Ensure all logs are written

```

Customizing the log directory, log level and format:
```csharp
using C1Logger; 
C1Log.LogDirectory = @"C:\Logs"; // Specify your log directory 
C1Log.MinLogLevel = C1Log.LogLevel.Warning; // Set minimum log level to Warning 
C1Log.Formatter = (dt, lvl, msg) => $"[{lvl.ShortHand()}] {dt:HH:mm:ss} - {msg}"; // Custom log message to use shorthand
C1Log.Info("Application started."); 
C1Log.Error("An error occurred."); 
C1Log.Flush(); // Ensure all logs are written
```

**Full API Example:**
```csharp
using System; 
using C1Logger;
public class MyCustomSink : C1Log.ILogSink {
    public C1Log.LogLevel MinLogLevel => C1Log.LogLevel.Notice; 
    public void Dispose() { } 
    public Task WriteAsync(string message, 
    C1Log.LogLevel level, IDictionary<string, object> context = null) {
        // Custom sink logic here 
        return Task.CompletedTask; 
        } 
    }
class Program { 
    static void Main() { 
        // Configure logger before first log
        C1Log.LogDirectory = @"C:\Logs"; 
        C1Log.MinLogLevel = C1Log.LogLevel.Notice; 
        C1Log.Formatter = (dt, lvl, msg) => $"[{lvl.ShortHand()}] {dt:HH:mm:ss} - {msg}"; 
        C1Log.AddSink(new MyCustomSink());
        // Subscribe to logging failures
        C1Log.OnLoggingFailure += ex => Console.Error.WriteLine($"Logging failed: {ex}");

        // Log messages
        C1Log.Info("Informational message.");
        C1Log.Warning("Warning message.");
        C1Log.Error("Error message.");
        C1Log.Exception(new Exception("Test exception"));
        C1Log.Flush(); // Ensure all logs are written
    }
}
```


---

## Building

If you want to build the project yourself, follow these steps:

1. Install [Visual Studio](https://visualstudio.microsoft.com/) (Community Edition is sufficient).
    - Select the **.NET desktop development** workload during installation.
2. Clone the repository to your local machine using Git or download it as a ZIP file.
3. Open the folder in Visual Studio as a new project.
4. Build the project by selecting **Build > Build Solution** or pressing `Ctrl + Shift + B`.
    - This should install all required dependencies. If not, manually install them via **Manage NuGet Packages**.
5. Once the build completes, you can run it:
    1. From Visual Studio: **Debug > Start Debugging** or `F5`
    2. From the built executable in `\bin\Debug\net8.0` (or `Release` for release builds).

---

## License

This project is licensed under the **Corpi1 Software License (Based on Mozilla Public License 2.0)**.

You are free to use, modify, and distribute the software. **Businesses and individuals can use it**, including in commercial activities. However, **you may not sell, repackage, or monetize the software itself**.

See the [LICENSE](LICENSE.md) file for full terms.