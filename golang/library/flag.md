go标准库flag源码分析
----

 1. 使用方式,flag库接受三种方式的参数输入
 	1. -param  param的值为bool型
 	2. -param=xxx param的值为任意实现了flag.value的类型
 	3. -param xx  param的值为非bool型，因为当值为bool型的时候可能会产生二义性，礼物xx=false,false可能是bool值也可能是个文件名

 2. 通过flag获取参数值的方式有两种(Type为实现了flag.value的类型)
 	1. flag.TypeVar()
 	2. flag.Type()

 3. flag是如何解析用户输入的参数的
 	1. 实例化flagSet
 	```
		var CommandLine = NewFlagSet(os.Args[0], ExitOnError)
		// NewFlagSet returns a new, empty flag set with the specified name and
        // error handling property.
        func NewFlagSet(name string, errorHandling ErrorHandling) *FlagSet {
        	f := &FlagSet{
        		name:          name,
        		errorHandling: errorHandling,
        	}
        	f.Usage = f.defaultUsage
        	return f
        }
	```
 	2. 把os.Args当做参数传给CommandLine进行解析 flag.Parse()
 	```
		func Parse() {
        	// Ignore errors; CommandLine is set for ExitOnError.
        	CommandLine.Parse(os.Args[1:])
        }
	```
 	3. 通过flagSet.Parse() 循环解析os.Args里面的参数
 	```
 	func (f *FlagSet) Parse(arguments []string) error {
    	f.parsed = true
    	f.args = arguments
    	//任何一次解析错误都会导致所有的参数值无法获取
    	for {
    		seen, err := f.parseOne()
    		if seen {
    			continue
    		}
    		if err == nil {
    			break
    		}
    		switch f.errorHandling {
    		case ContinueOnError:
    			return err
    		case ExitOnError:
    			os.Exit(2)
    		case PanicOnError:
    			panic(err)
    		}
    	}
    	return nil
    }

	```
	2. flagSet.parseOne()//逐个解析参数的过程
	```
		// parseOne parses one flag. It reports whether a flag was seen.
        func (f *FlagSet) parseOne() (bool, error) {
        	//没有传递参数，直接退出
        	if len(f.args) == 0 {
        		return false, nil
        	}
        	s := f.args[0]
        	//参数
        	//或参数不是以字符"-"开头的情况
        	//或者参数只有一个字符
        	//这三种情况都退出解析
        	if len(s) == 0 || s[0] != '-' || len(s) == 1 {
        		return false, nil
        	}
        	numMinuses := 1
        	//参数以"--"开头的情况(不建议，多了要处理的情况)
        	if s[1] == '-' {
        		numMinuses++
        		//参数有且只有两个"-"的情况，退出
        		if len(s) == 2 { // "--" terminates the flags
        			f.args = f.args[1:]
        			return false, nil
        		}
        	}
        	//获得参数名称
        	name := s[numMinuses:]
        	//参数名称为空
        	//参数名称里面还有"-"
        	//参数名称以"="(也是没有参数名的情况)
        	//这三种情况也退出
        	if len(name) == 0 || name[0] == '-' || name[0] == '=' {
        		return false, f.failf("bad flag syntax: %s", s)
        	}
        
        	// it's a flag. does it have an argument?
        	f.args = f.args[1:]//注意这里的f.args的变化
        	hasValue := false
        	value := ""
        	//针对传递参数的第二种情况(通过"="传递),获取参数的值
        	for i := 1; i < len(name); i++ { // equals cannot be first
        		if name[i] == '=' {
        			value = name[i+1:]
        			hasValue = true
        			name = name[0:i]
        			break
        		}
        	}
        	m := f.formal//f.formal里面是用户绑定的参数
        	flag, alreadythere := m[name] // BUG
        	//发现解析出来的参数名称并没有被绑定，则报错
        	if !alreadythere {
        		if name == "help" || name == "h" { // special case for nice help message.
        			f.usage()
        			return false, ErrHelp
        		}
        		return false, f.failf("flag provided but not defined: -%s", name)
        	}
        
        	//针对第一种传递参数的情况获取参数值
        	if fv, ok := flag.Value.(boolFlag); ok && fv.IsBoolFlag() { // special case: doesn't need an arg
        		if hasValue {
        			if err := fv.Set(value); err != nil {
        				return false, f.failf("invalid boolean value %q for -%s: %v", value, name, err)
        			}
        		} else {
        			if err := fv.Set("true"); err != nil {
        				return false, f.failf("invalid boolean flag %s: %v", name, err)
        			}
        		}
        	} else {
        		//对第三种传递参数的方式获取参数值
        		// It must have a value, which might be the next argument.
        		if !hasValue && len(f.args) > 0 {
        			// value is the next arg
        			hasValue = true
        			value, f.args = f.args[0], f.args[1:] //注意这里的f.args的变化
        		}
        		if !hasValue {
        			return false, f.failf("flag needs an argument: -%s", name)
        		}
        		if err := flag.Value.Set(value); err != nil {
        			return false, f.failf("invalid value %q for flag -%s: %v", value, name, err)
        		}
        	}
        	if f.actual == nil {
        		f.actual = make(map[string]*Flag)
        	}
        	f.actual[name] = flag  //设置实际获取到的flag
        	return true, nil
        }
	```
	
4. flag包里面几个重要的数据结构
	1. value,实现了value接口的类型可以被flag.Var()获取到  
		```
		type Value interface {
			String() string
			Set(string) error
		}
		``` 
	
	2. flag  
	
		```
		// A Flag represents the state of a flag.
		type Flag struct {
			Name     string // name as it appears on command line
			Usage    string // help message
			Value    Value  // value as set
			DefValue string // default value (as text); for usage message
		}
		```	  
	
	3. flagSet  
	
		```
		type FlagSet struct {
			// Usage is the function called when an error occurs while parsing flags.
			// The field is a function (not a method) that may be changed to point to
			// a custom error handler.
			Usage func()
		
			name          string
			parsed        bool
			actual        map[string]*Flag //保存用户实际传递的参数
			formal        map[string]*Flag //保存用户绑定的参数
			args          []string // arguments after flags //保存解析后剩余的参数
			errorHandling ErrorHandling
			output        io.Writer // nil means stderr; use out() accessor
		}
		```
	
	4. 通过实现value接口让flag获取一种新的参数类型
	
		```
		type sliceValue []string
		
		func newSliceValue(vals []string, p *[]string) *sliceValue {
			*p = vals
			return (*sliceValue)(p)
		}
		
		func (s *sliceValue) Set(val string) error {
			*s = sliceValue(strings.Split(val, ","))
			return nil
		}
		
		func (s *sliceValue) Get() interface{} { return []string(*s) }
		
		func (s *sliceValue) String() string { return strings.Join([]string(*s), ",") }
		
		//使用方式
		var languages []string
		flag.Var(newSliceValue([]string{}, &languages), "slice", "I like programming `languages`")
		//这样通过 -slice "go,php" 这样的形式传递参数，languages 得到的就是 [go, php]
		```