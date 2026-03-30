- overleaf 
	- https://github.com/iamhyc/Overleaf-Workshop/wiki
	- you need to log in with your browser, then use View>Developer>Network to get the cookies from the header 
- vim so it wraps visually (settings.json)
~~~
{
    "vim.normalModeKeyBindingsNonRecursive": [
        {
            "before": ["j"],
            "after": ["g", "j"]
        },
        {
            "before": ["k"],
            "after": ["g", "k"]
        }
    ]
}
~~~