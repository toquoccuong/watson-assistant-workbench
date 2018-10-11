## Dialog

### Localization

You may want to make your xml dialog language independent, i. e. use the same dialog structure for different languages.

##### Encoding
You can replaces all language-dependent fields with codes using the script `dialog_text2code` which creates a resource file with translations of codes. E. g. running

```
python scripts/dialog_text2code.py "example/en_app/dialogs/chitchat.xml" "chitchat-resource-en.json" -o "chitchat-encoded.xml" -v

```

will replace all values of `<values>` and `<text>` tags in the input file `chitchat.xml` with codes and creates a `chitchat-resource-en.json` file with translations.


###### Input

`chitchat.xml` file

```
...
	<output>
		<textValues>
			<values>My name is $botName. What is your name?</values>
		</textValues>
	</output>
...
```

###### Output

`chitchat-encoded.xml` file

```
...
	<output>
		<textValues>
			<values>%%TXT17</values>
		</textValues>
	</output>
...
```

`chitchat-resource-en.xml` file

```
{
...
	"TXT17": "My name is $botName. What is your name?",
...
}
```


You can set dfferent prefix of code using the `-p CHITCHAT_` switch and it is also possible to add more tags to be replaced by specifying the `-t` tag. E. g. command

```
python scripts/dialog_text2code.py "example/en_app/dialogs/chitchat.xml" "chitchat-resource-en.json" -o "chitchat-encoded.xml" -p "CHITCHAT_" -t "//text[not(values)]" "//values" "//condition" -v

```

will replace all `text` tags which has no `values` subtags, all `values` tags and all `condition` tags with codes prefixed by "CHITCHAT_".

###### Input

`chitchat.xml` file

```
...
	<condition>#ALL_ABOUT_ME_WHAT_IS_YOUR_NAME or input.text.contains('name')</condition>
...
```

###### Output

`chitchat-encoded.xml` file

```
...
	<condition>%%CHITCHAT_7</condition>
...
```

`chitchat-resource-en.json` file

```
{
...
	"CHITCHAT_7": "#ALL_ABOUT_ME_WHAT_IS_YOUR_NAME or input.text.contains('name')",
...
}
```

##### Decoding
Having `chitchat-encoded.xml` file and e. g. `chitchat-resource-cz.json` file with czech translations you can create czech version of source dialog using command

```
python scripts/dialog_code2text.py "chitchat-encoded.xml" "chitchat-resource-cz.json" -o "chitchat-cz.xml" -t "//text[not(values)]" "//values" "//condition" -v
```

###### Input

`chitchat-encoded.xml` file

```
...
	<condition>%%CHITCHAT_7</condition>
...
	<output>
		<textValues>
			<values>%%CHITCHAT_17</values>
		</textValues>
	</output>
...
```

`chitchat-resource-cz.xml` file

```
{
...
	"CHITCHAT_7": "#ALL_ABOUT_ME_WHAT_IS_YOUR_NAME or input.text.contains('jméno')",
...
	"CHITCHAT_17": "Jmenuji se $botName. Jak se jmenuješ ty?",
...
}
```

###### Output

`chitchat-cz.xml` file

```
...
	<condition>#ALL_ABOUT_ME_WHAT_IS_YOUR_NAME or input.text.contains('jméno')</condition>
...
	<output>
		<textValues>
			<values>Jmenuji se $botName. Jak se jmenuješ ty?</values>
		</textValues>
	</output>
...
```