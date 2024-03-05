
### Constructor
```
Rectangle::Rectangle(double width, double height) : width_(width), height_(height) {
	if (width_ <= 0 || height_ <= 0) {
		throw std::invalid_argumnet();
	}
}
```
### Map
```
std::string StrToLower(std::string str) {
	std::string str_lower;
	for (char c: str) {
		str_lower += std::tolower(c);
	}
	return str_lower;
}

std::string StrRemoveP(std::string str) {
	std::string str_nop;
	for (char c: str) {
		if (c != '.' && c != ',' && c != '?' && c != '!') {
			str_nop += c;
		}
	}
	return str_nop;
}

void StringToWord(std::string str, std::map<std::string, unsigned int> frequencyMap) {
	std::string word;
	for (char c : str) {
		if (c != ' ') {
			word += c;
		} else if (!word.empty()) {
			frequencyMap[word]++;
			word.clear();
		}
	}
	if (!word.empty()) {
		frequencyMap[word]++;
		word.clear();
	}
}

std::map<std::string, unsigned int> WordFrequencyCounter(std::string str) {
	std::string str_lower = StrToLower(str);
	std::string str_nop = StrRemoveP(str_lower);
	std::map<std::string, unsigned int> frequencyMap;
	StringToWord(str_nop, frequencyMap);
	return frequencyMap;
}
```

```
std::string ReplaceForbiddenWords(const std::string& input, const std::map<std::string, std::string>& forbiddenWords) {
	std::string result;
	std::string word;

	for (char c : input) {
		if (c != ' ') {
			word += c;
		} else if (!word.empty()) {
			if (forbiddenWords.count(word) > 0) {
				result += forbiddenWords.at(word); //find value at key
			} else {
				result += word;
			}
		}
	}
	if (!word.empty()) {
		if (forbiddenWords.count(word) > 0) {
			result += forbiddenWords.at(word); //find value at key
		} else {
			result += word;
		}
	}
	return result;
}
```

### Git
update: git add -u
commit: git commit -m
pus: git push

###  Compilation Pipeline
1. Preprocessor -> temporary file
2. Compiler -> assembler file
3. Assembler -> object file
4. Linker -> executable file

1. -E: temporary file (preprocess)
2. -S: assembler output file (preprocess+compile)
3. -c: object file (preprocess+compile+assemble)

```
std::string word;
for (char c : input) {
	if (c != ' ') {
		word += c;
	} else if (!word.empty()) {
		if (map.contains(word)) {
			redcate += map.at(word);
		} else {
			redcate += word;
		}
		redcate += ' ';
		word.clear();
	}
}
if (!word.empty()) {
	if (map.contains(word)) {
		redcate += map.at(word);
	} else {
		redcate += word;
	}
	redcate += ' ';
}

```