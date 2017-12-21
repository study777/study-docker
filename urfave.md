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
        app := cli.NewApp()  //返回一个 *App  下面是赋值操作
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

```
go doc  github.com/urfave/cli Command
```

```golang
type Command struct {
    // The name of the command
    Name string
    // short name of the command. Typically one character (deprecated, use `Aliases`)
    ShortName string
    // A list of aliases for the command
    Aliases []string
    // A short description of the usage of this command
    Usage string
    // Custom text to show on USAGE section of help
    UsageText string
    // A longer explanation of how the command works
    Description string
    // A short description of the arguments of this command
    ArgsUsage string
    // The category the command is part of
    Category string
    // The function to call when checking for bash command completions
    BashComplete BashCompleteFunc
    // An action to execute before any sub-subcommands are run, but after the context is ready
    // If a non-nil error is returned, no sub-subcommands are run
    Before BeforeFunc
    // An action to execute after any subcommands are run, but after the subcommand has finished
    // It is run even if Action() panics
    After AfterFunc
    // The function to call when this command is invoked
    Action interface{}

    // Execute this function if a usage error occurs.
    OnUsageError OnUsageErrorFunc
    // List of child commands
    Subcommands Commands
    // List of flags to parse
    Flags []Flag
    // Treat all flags as normal arguments if true
    SkipFlagParsing bool
    // Skip argument reordering which attempts to move flags before arguments,
    // but only works if all flags appear after all arguments. This behavior was
    // removed n version 2 since it only works under specific conditions so we
    // backport here by exposing it as an option for compatibility.
    SkipArgReorder bool
    // Boolean to hide built-in help command
    HideHelp bool
    // Boolean to hide this command from help or completion
    Hidden bool
    // Boolean to enable short-option handling so user can combine several
    // single-character bool arguements into one
    // i.e. foobar -o -v -> foobar -ov
    UseShortOptionHandling bool

    // Full name of command for help, defaults to full command name, including parent commands.
    HelpName string

    // CustomHelpTemplate the text template for the command help topic.
    // cli.go uses text/template to render templates. You can
    // render custom help text by setting this variable.
    CustomHelpTemplate string
    // Has unexported fields.
}
    Command is a subcommand for a cli.App.
```


```golang

app.Before = func(context *cli.Context) error {
                // Log as JSON instead of the default ASCII formatter.
                log.SetFormatter(&log.JSONFormatter{})

                log.SetOutput(os.Stdout)
                return nil
        }

type App struct {
    Before BeforeFunc
}

go doc  github.com/urfave/cli BeforeFunc

type BeforeFunc func(*Context) error
    BeforeFunc is an action to execute before any subcommands are run, but after
    the context is ready if a non-nil error is returned, no subcommands are run
    
    




```
