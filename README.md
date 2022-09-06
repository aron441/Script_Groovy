# Script_Groovy
Search and Replace String Using Groovy Script
        /*
        *
        * @author  Aron Malabao
        * @date    08-03-2022
        * @version 0.3.1
        */
        
        //The two lines after that enable the script to create a beautiful GUI for the text interface. A Groovy class called SwingBuilder is used to create GUIs almost         as quickly as HTML forms.
        import groovy.swing.SwingBuilder
        
        //The SearchReplaceForm class is present to store the form's data.Two fields, search and replace, represent the text to be searched for and changed,                    respectively. The SwingBuilder component 
        uses these fields to update the variables when the user makes changes to them in the GUI.
        import java.awt.BorderLayout as BL
        
        // The @Bindable annotation is required to take into accounts the values in the form 
        import groovy.beans.Bindable
        
        // The SearchReplaceForm class is here to hold the data from the form. Search and replace are two fields representing the text to search and to                         replace,respectively. These fields are used by the SwingBuilder component which will.Update the variables when the user changes them in the GUI. 
        @Bindable
        class FormData { String search, replace }
        
        // You can use these two variables to pre-define the values of the search and replace fields
        def search_string  = ""
        def replace_string = ""
        
        def formData = new FormData()
        formData.search = search_string
        formData.replace = replace_string

        //Currently, the doReplace function does nothing on its own. We'll create a basic graphical user interface (GUI) with a button to start the replacement and a           field for the user to enter search and replace strings
        new SwingBuilder().edt {
        frame(title:res.getString("name"), size: [350, 200], show: true) {
        borderLayout(vgap: 5
        
        panel(constraints: BL.CENTER,
        border: compoundBorder([
            emptyBorder(10),
            titledBorder(res.getString("description"))
        ])) {
        
        //We can use a Table layout To with labels and Textfields, just like in an HTML form
            tableLayout {
                tr {
                    td { label res.getString("search") }
                    td { textField id:"searchField", text:formData.search, columns: 20  }
                }
                tr {
                    td { label res.getString("replace") }
                    td { textField id:"replaceField", text:formData.replace, columns: 20  }
                }
                tr {
                    td { label "" }
                    td {
                        // When the button is clicked, the doReplace function will be called
                        button(text:res.getString("button"),
                        actionPerformed: {
                            doReplace(formData.search, formData.replace)
                        },
                        constraints:BL.SOUTH)
                    }
                }
            }

             // Binding of textfields to the formData object.
                bean formData,
                    search:  bind { searchField.text },
                    replace: bind { replaceField.text }
        }
    }
}


    //A Groovy function is called doReplace. It will carry out the real search and text replacement job. The segment count variable will be increased each time a segment is modified. It should be noted that the search string and replace string are parameters of the function and are not the same as the two previously declared variables
    def doReplace(search_string, replace_string) {
    def segment_count = 0

        //An item from the OmegaT application is called "console." At the base of the script interface, it represents the logging window. Only the three methods print,         println, and clear are available. The ResourceBundle called "res" is used to translate the script into several languages.
        console.println(res.getString("description"));
    
        //Similar to "console," "project" is bound to OmegaT. This is the project that is presently open. The all Entries member returns all of the project's segments,         and each segment is processed by a Groovy function between... }. "ste" is the name of the currently 
        processed segment (for SourceTextEntry)
 
        project.allEntries.each { ste ->
        // Each segment has a source text, stored here in the source variable.
        source = ste.getSrcText();
        
        // If the segment has been translated, we get store translated text in the target variable.
        target = project.getTranslationInfo(ste) ? project.getTranslationInfo(ste).translation : null;
        
        //To compare the translated text before and after text replacement and establish whether the segment was altered, a copy of the translated text is made
        initial_target = target

        // Skip untranslated segments
        if (target == null) return

       //In the translated text, the replace string takes the place of the search string 
        target = target.replaceAll(search_string, replace_string)

     
        //If the two translations don't match, we move on to the next part and substitute the new translation for the previous one.The object known as "editor" is used to modify the primary OmegaT user interface..
        if (initial_target != target) {
            segment_count++
            
            // Jump to the segment number
            editor.gotoEntry(ste.entryNum())
            console.println(ste.entryNum() + "\t" + ste.srcText + "\t" + target )
           
            // Replace the translation
            editor.replaceEditText(target)
        }
    }

    console.println(res.getString("modified_segments") + segment_count);


    NOTE: Because the groovy class returns an error when expecting an EOF, I discovered '? ' @ line 9, column 25, I'm using the following Groovy code to generate a   random number. I can test it in, say, Groovy Web Console (https://groovyconsole.appspot.com/) and it works, but when I try to run it in, it fails.
    
          Line 25 contains some extra unicode characters. When you convert it to hex, you get: 64 65 66 20 72 61 6e 20 3d 20 .....
          Now if you convert this hex back to ascii, you will get : def ran = Math.abs(â€‹ranInt)â€‹%20â€‹0;

        Code: log.info ">>run"
              Random random = new Random()
              def ranInt = random.nextInt()
              def ran = Math.abs(​ranInt)​%20​0;
              log.info ">>sleep counter:"+flowVars.counter+" ran: "+ran
              sleep(ran)
