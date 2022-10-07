# **THML Handwritten Markup Language**
<img src="THMarkup.jpg" height="100" width = "100"/> \
[[简体中文](Readme.zh_CN.md)|[繁體中文](Readme.zh_TW.md)] \
THML is a markup language whice only uses 4 characters as basic characters - colon (:), full stop (.), backslash (\\) and semicolon (;). It focuses on writability and the degree freedom. \
The current version is v0.1.0, still in develop period, provides basic functons only.
## Catalog
* [Spec](#Spec)
* [Object Type](#Object-Type)
* [Text](#Text)
* [Comments](#Comments)
* [Indentation Control](#Indentation-Control)
* [Extension](#Extension)
* [Available Parsers](#Available-Parsers)

## Spec
* A THML docment must be a valid UTF-8 encoded Unicode document.
* THML is case-sensitive.
* Whitespace means space (0x20) and tab (0x19)
* Linefeed means LF (0x0A), CR (0x0D) or CRLF (0x0D0A)
## Object Type
Object type includes Document, Object, List and ListItem.
### Document
Document is the root object and usually embodied in parser.
### Object
Object usually appears as key/value pair, the form is `key:value;` . Key and value are divided by colon, the value is between colon and semicolon.\
Key name must be text. There's no limit as long as followingting the rule. The detail rule see [Text](#Text).

    name: clear; \..Valid
    一二: 三四; \..Valid
    \ \: empty; \..Valid but discouraged
    one two: three; \..Invalid, not follow the rule
Value can be text, one or more object or list. Text and objects cannot exist at the same time.

    store:
        owner: Mike;
        fruit: apple.. orange.. banana;
    ;
Duplication of name is allowed.

    number: one;
    number: two;
To express a text array please use [List and ListItem](#List-and-ListItem).

### List and ListItem
The children of List must be ListItem. ListItem's parent must be List, too.\
The name rule of List and common Object is same. Naming a ListItem is not allowed, and the rule of value as well as common Object.\
Different ListItems are divided by `..` .

    list:
        item1: 1; ..
        2..
        item3: 3; item3.2: 3.2;
    ;
If List only has one item, please decleare the List with `..` after the name.

    singleItem..: single;
If the list is empty, the parser would suppose it contains a empty item.

    emptyList..: ;
## Text
The type of text contains Normal, Single and Multiple.\
All text type supports multi-line.
The rule of comments and indentation in the text see [Comments](#Comments) and [Indentation Control](#Indentation-Control).
### Normal
Normal type text has no begin and end sign. The whitespaces before and after the text are ignored.

    \.The meanings of these objects are same.\
    foo: bar;
    foo:
        bar
    ;
Normal type text follows Comment rule but not follows indentation rule. Every spaces at the head of line is ignored.

    \.The meanings of these objects are same.\
    multiline:
        1
        2
        3
    ;
    multiline:
        1\.Have comment.\
        2
        3
    ;
    multiline:
    1
        2
            3
    ;
If writing colon, backslash and semicolon, please escape them. (`\:`, `\\` and `\;`)If writing `..`, please use Single and Multiple type.\
Escaping LF, CR and CRLF (`\.\`, `\.:\` and `\.;\`) (The method of escaping CR and CRLF will be likely simplefiled in the later version).\
Comments, whitespaces and unescaped linefeed are not allowed in Normal-typed name.
### Single
Single type text uses a backslash as begin and end sign. To prevent ambiguity(e.g. the special character after begin sign) and make convenience for multi-line writing, the first whitespace or linefeed after the begin sign and the first linefeed before the end sign are ignored.

    spaceOnly: \  \; \..Space
    empty: \ \; \..empty

    \.The meanings of these objects are same.\
    foo: \bar\;
    foo: \ bar\;
    foo: \
        bar
        \
    ;
Singe type text followes comment rule and indentation rule.

    \.The meanings of these objects are same.\
    foo: \
        bar\..have
        Blat
        \
    ;
    foo: barBlat;

    \.According to the indentation rule, their meanings are different.\
    foo: \
        bar
        \
    ;
    foo: \
        bar
    \
    ;
Single type text supports escaping backslash and linefeed, but colon and semicolon are unsupported.
### Multiple
Multiple type uses same over 3 odd number of backslashes as begin and end sign. To prevent ambiguity(e.g. backslashes after begin sign) and make convenience for multi-line writing, the first whitespace or linefeed after the begin sign and the first whitespace or linefeed are ignored.

    empty: \\\ \\\;
    empty: \\\  \\\; \..Both empty text

    \.The meanings of these objects are same.\
    foo: \\\bar\\\;
    foo: \\\ bar \\\;
    foo: \\\
        bar
        \\\
    ;
Comments and escapes are not supported in Multiple type text, but it supports indentation rule. It can store and write documents with indentations better, while not changes informations and layout.

    C_Language: \\\
        #include <stdio.h>

        int main()
        {
            printf("Hello\n");
            return 0;
        }
        \\\
    ;
It can help write indentation-sensitive languages, such as Python and YAML.\
If the text has same backslashes as well as begin and end sign, please change the length of begin and end sign.

    Triple: \\\ \\\ \\\; \..Invalid
    Triple: \\\\\ \\\ \\\\\; \..Valid
## Comments
Comments can be inserted before name, between name and colon, begin and end of value, Normal type text as value and any Single type text.\
Line comment appears as `\..` . If the comment in text, it would cancel current linefeed.\
Block comment appears as `\. .\`.

    \..This is a line comment
    \.This is a block comment.\
## Indentation Control
THML regulates a tab contains no more than 4 spaces. 4 spaces length as a group of alignment. If hte position cannot be exactly divided by 4, it would alignes the longest position can be exactly divided by 4 less than current position.\
Alignment is ensured by the main part, comment, begin and end sign of the text. It usually takes the closest position of them, therefore the text aligns that position.

    t1:
        \
        tab1
        \
    ;
    t2:
        \
    tab2
        \
    ;
    t3:
    \
        tab3
    \
    ;
    t4:
        \
    \..comment
        tab4
        \
    ;
The alignment position of t2, t3 and t4 ae same, no spaces away from the head of line.\
Putting the begin and end sign after the text is discouraged.
If it has other characters before befin sign, the sign not calculates the alignment.

    \.The meanings of these objects are same.\
    foo: \
        bar
        \
    ;
    foo:
        \
        bar
        \
    ;
## Extension
Please use `.th` as extension.
## Available Parsers
* C# : [My shit code: THMarkup](https://github.com/l-thoms/THMarkup) \
Welcome to write more high quality parsers.
## ABNF
Coming soon.