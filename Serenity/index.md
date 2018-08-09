# 使用经验

## CodeGenerationHelpers.ttinclude  生成的代码的黑屏停留设置

```c#
    public void Compile(string workingDirectory, string arguments) {
        var tscPath = DetermineTSCPath();

        var psi = new System.Diagnostics.ProcessStartInfo(tscPath, arguments);
		psi.WorkingDirectory = workingDirectory;
        System.Diagnostics.Process.Start(psi).WaitForExit(10000);

			//var process = new System.Diagnostics.Process();
            //process.StartInfo.FileName = "cmd";
            //process.StartInfo.UseShellExecute = false;
            //process.StartInfo.Arguments = "/k \""+ tscPath +"\"";
			//process.StartInfo.WorkingDirectory=workingDirectory;
            //process.Start();

    }
```