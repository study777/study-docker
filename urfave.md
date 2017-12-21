```golang
package main

import (
        log "github.com/Sirupsen/logrus"
        "github.com/urfave/cli"
        "os"
)

const usage = `mydocker is a simple container runtime implementation.
                           The purpose of this project is to learn how docker works and how to write a docker by ourselves
                           Enjoy it, just for fun.`

func main() {
        app := cli.NewApp()
        app.Name = "mydocker"
        app.Usage = usage

        app.Commands = []cli.Command{
                initCommand,
                runCommand,
        }

        app.Before = func(context *cli.Context) error {
                // Log as JSON instead of the default ASCII formatter.
                log.SetFormatter(&log.JSONFormatter{})

                log.SetOutput(os.Stdout)
                return nil
        }

        if err := app.Run(os.Args); err != nil {
                log.Fatal(err)
        }
}
                                                                                                                                                     34,1          Bot
```

```
godoc -src  github.com/urfave/cli  NewApp
```

```golang
func NewApp() *App {
    return &App{
        Name:         filepath.Base(os.Args[0]),
        HelpName:     filepath.Base(os.Args[0]),
        Usage:        "A new cli application",
        UsageText:    "",
        Version:      "0.0.0",
        BashComplete: DefaultAppComplete,
        Action:       helpCommand.Action,
        Compiled:     compileTime(),
        Writer:       os.Stdout,
    }
}
```


```
godoc -src  github.com/urfave/cli  App
```


```golang
type App struct {
    // The name of the program. Defaults to path.Base(os.Args[0])
    Name string
    // Full name of command for help, defaults to Name
    HelpName string
    // Description of the program.
    Usage string
    // Text to override the USAGE section of help
    UsageText string
    // Description of the program argument format.
    ArgsUsage string
    // Version of the program
    Version string
    // Description of the program
    Description string
    // List of commands to execute
    Commands []Command
    // List of flags to parse
    Flags []Flag
    // Boolean to enable bash completion commands
    EnableBashCompletion bool
    // Boolean to hide built-in help command
    HideHelp bool
    // Boolean to hide built-in version flag and the VERSION section of help
    HideVersion bool
    // Populate on app startup, only gettable through method Categories()
    categories CommandCategories
    // An action to execute when the bash-completion flag is set
    BashComplete BashCompleteFunc
    // An action to execute before any subcommands are run, but after the context is ready
    // If a non-nil error is returned, no subcommands are run
    Before BeforeFunc
    // An action to execute after any subcommands are run, but after the subcommand has finished
    // It is run even if Action() panics
    After AfterFunc

    // The action to execute when no subcommands are specified
    // Expects a `cli.ActionFunc` but will accept the *deprecated* signature of `func(*cli.Context) {}`
    // *Note*: support for the deprecated `Action` signature will be removed in a future version
    Action interface{}

    // Execute this function if the proper command cannot be found
    CommandNotFound CommandNotFoundFunc
    // Execute this function if an usage error occurs
    OnUsageError OnUsageErrorFunc
    // Compilation date
    Compiled time.Time
    // List of all authors who contributed
    Authors []Author
    // Copyright of the binary if any
    Copyright string
    // Name of Author (Note: Use App.Authors, this is deprecated)
    Author string
    // Email of Author (Note: Use App.Authors, this is deprecated)
    Email string
    // Writer writer to write output to
    Writer io.Writer
    // ErrWriter writes error output
    ErrWriter io.Writer
    // Execute this function to handle ExitErrors. If not provided, HandleExitCoder is provided to
    // function as a default, so this is optional.
    ExitErrHandler ExitErrHandlerFunc
    // Other custom info
    Metadata map[string]interface{}
    // Carries a function which returns app specific info.
    ExtraInfo func() map[string]string
    // CustomAppHelpTemplate the text template for app help topic.
    // cli.go uses text/template to render templates. You can
    // render custom help text by setting this variable.
    CustomAppHelpTemplate string

    didSetup bool
}


```