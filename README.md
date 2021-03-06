easyccg
=======

EasyCCG is a CCG parser created by Mike Lewis.

If you use EasyCCG in your research, please cite the following paper:

> _A* CCG Parsing with a Supertag-factored Model_, Mike Lewis and Mark Steedman, EMNLP 2014.

## Models

Pre-trained models are available from [here](https://drive.google.com/#folders/0B7AY6PGZ8lc-NGVOcUFXNU5VWXc). Unpack the `.tar.gz` file after downloading.

To train new models, follow the instructions in [training/README](training/README).

## Usage

### Basic Usage

~~~bash
java -jar easyccg-all.jar --model model
~~~

**Notes:**

- The `--model` option is mandatory
- You can use the short form `-m` instead of `--model`
- The value for `--model` must be the directory of a model downloaded and unpacked in [Models](#models)
- After the model has been loaded, you can enter sentences in standard input
- When you're finished, hit *Ctrl-D*

### File Input

Read sentences from a file rather than from standard input.

~~~bash
java -jar easyccg-all.jar -m model -f file
~~~

**Notes:**

- The value for the `-f` option must be a text file with sentences separated by newlines
- The long form of `-f` is `--inputFile`


### Output Formats

~~~bash
java -jar easyccg-all.jar -m model -f file -o format
~~~

`format` must be one of:

- `ccgbank` (default)
- `html`
- `supertags`
- `deps`
- `extended` (requires `-i POSandNERtagged`)
- `prolog` (requires `-i POSandNERtagged`)


### Help

List all command line options.

~~~bash
java -jar easyccg-all.jar --help
~~~

### Advanced Usage

For N-best parsing:

~~~bash
java -jar easyccg-all.jar --model model --nbest 10
~~~

To parse questions, use:

~~~bash
java -jar easyccg-all.jar --model model_questions -s -r S[q] S[qem] S[wq]
~~~

If you want POS/NER tags in the output, you'll need to supply them in the input, using the format `word|POS|NER`. To get this format from the C&C tools, use the following:

~~~bash
echo "parse me" | candc/bin/pos --model candc_models/pos | candc/bin/ner -model candc_models/ner -ofmt "%w|%p|%n \n" | java -jar easyccg-all.jar --model model_questions -i POSandNERtagged -o extended
~~~

To get Boxer-compatible Prolog output, use:

~~~bash
echo "parse me" | candc/bin/pos --model candc_models/pos | candc/bin/ner -model candc_models/ner -ofmt "%w|%p|%n \n" | java -jar easyccg-all.jar --model model -i POSandNERtagged -o prolog -r S[dcl]
~~~

## Compile

~~~
./gradlew build
~~~

This creates `build/libs/easyccg-all.jar`, which is a self-contained executabyle JAR file.
