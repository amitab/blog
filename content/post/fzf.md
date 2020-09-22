+++
title = "FZF all the things!"
date = "2017-07-28T18:00:00+07:30"
tags = ["fzf", "plugin"]
draft = false
author = "Amitabh Das"
+++

For those who don't know what it is, I suggest you check [this](https://github.com/junegunn/fzf.vim) out.
It is more or less a tool to filter out lines, and it does that fast. I used it mainly for what the ready made script was for until I realized that I could replace many vim plugins with this!

# Replacing the project search:

Ok, to be fair the fzf.vim comes with the search integration. But you can extend this to be far more powerful to your workflow than you can imagine!

The method `fzf#vim#ag()` is provided to execute a search through the Silver Searcher.

Now wouldn't it be awesome if you could just press a combination of keys, input your search query, and have your results pop up which can then be filtered through the fuzzy search?

```viml
function! Search()
    call inputsave()
    let query = input('Search: ')
    call inputrestore()
    if query != ""
      call fzf#vim#ag(query, '--color-path "1;36"', fzf#vim#with_preview())
    else
      echom "Cancelled Search!"
    endif
endfunction
```

_How about searching for the word under the cursor?_

```viml
function! SearchWordWithAg()
  execute 'Ag' expand('')
endfunction
```

_That would be a single word though. How about a visual selection?_

```viml
function! SearchVisualSelectionWithAg() range
  let old_reg = getreg('"')
  let old_regtype = getregtype('"')
  let old_clipboard = &clipboard
  set clipboard&
  normal! ""gvy
  let selection = getreg('"')
  call setreg('"', old_reg, old_regtype)
  let &clipboard = old_clipboard
  execute 'Ag' selection
endfunction
```

# Replacing the code browsing tools

I don't know about you all, but I'm not a big fan of the cscope UI. So how about we use FZF to replace cscope?

```viml
function! Cscope(option, query)
  let color = '{ x = $1; $1 = ""; z = $3; $3 = ""; printf "\033[34m%s\033[0m:\033[31m%s\033[0m\011\033[37m%s\033[0m\n", x,z,$0; }'
  let opts = {
  \ 'source':  "cscope -dL" . a:option . " " . a:query . " | awk '" . color . "'",
  \ 'options': ['--ansi', '--prompt', '> ',
  \             '--multi', '--bind', 'alt-a:select-all,alt-d:deselect-all',
  \             '--color', 'fg:188,fg+:222,bg+:#3a3a3a,hl+:104'],
  \ 'down': '40%'
  \ }
  function! opts.sink(lines) 
    let data = split(a:lines)
    let file = split(data[0], ":")
    execute 'e ' . '+' . file[1] . ' ' . file[0]
  endfunction
  call fzf#run(opts)
endfunction
```

fzf.vim provides us with just the way to pull that off! The function `fzf#run()` accepts a dictionary with a source and a sink. Where the source would be the cscope command you run. In my case, I'm running cscope to return a list of matches and then using awk to print them in a pretty format. The sink would be the execution on selection of a result item.
All I'm doing here is executing the `:e +` command.

Voila! You now have a cscope result list (Meh) which can be filtered using keywords (Wat!) Just map it to whatever keys that makes sense to you.

# Conclusion

You could probably do a lot more, imagination is your limit! Lighten you vim plugin list!
