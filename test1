from pyparsing import Literal, OneOrMore, Word, alphanums, SkipTo

def format_chat_template(text: str) -> str:
    # Define the grammar
    word = Word(alphanums)
    dot = "."
    comma = ","
    apostrophe = "'"
    gen_command = Literal("{{gen") + SkipTo("}}") + Literal("}}")
    grammar = OneOrMore(word | dot | gen_command | comma | apostrophe)

    #parse the string
    parsed_text = grammar.parseString(text)

    # Format the text
    formatted_text = "{{#user}} "
    for x,token in enumerate(parsed_text):
        if token.startswith("{{gen"):
            formatted_text +="{{/user}} {{#assistant}} "
        formatted_text += token + " "
        if token.startswith("}}"):
              if x == len(parsed_text) - 1:
                  formatted_text += "{{/assistant}} "
              else:
                formatted_text += "{{/assistant}} {{#user}} "
        if parsed_text[-1] == token:
             if "{{/user}}" != formatted_text:
                 formatted_text += "{{/user}} "

    # Ensure the template ends with the assistant's {{gen 'write' }} command
    if not formatted_text.endswith("{{/assistant}} "):
          formatted_text += "{{#assistant}}{{gen 'write' }}{{/assistant}} "

    return formatted_text

# Test examples
text1 = "Tweak this proverb to apply to model instructions instead. Where there is no guidance{{gen 'rewrite'}}"
text2 = "how are things going, tell me about Delhi"
text3 = "Hello {{gen How are you?}} I'm fine {{gen That's good to hear}}"
text4 = "I'm going home, please lock the door.{{gen sure,good night}}"

formatter = format_chat_template(text1)  #pass test cases(text1 , text2 , text3 , text4) as argument
print(formatter)
