import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.print.PrinterException;
import java.io.*;

public class simpleTextEditor extends JFrame implements ActionListener {
    JFrame Frame;
    JTextArea Textarea;
    simpleTextEditor(){
        Frame=new JFrame("simple Text Editor"); //created frame
        Textarea=new JTextArea(); //created text area
        Frame.add(Textarea); // adding text area to frame
        Frame.setSize(800,800); //set frame size
        Frame.setVisible(true);
        Frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE); //frame action

        JMenuBar menuBar = new JMenuBar(); //creating menu bar
        JMenu fileMenu = new JMenu("File menu"); //creating file menu
        JMenu editMenu = new JMenu("Edit menu"); //creating edit menu
        menuBar.add(fileMenu);
        menuBar.add(editMenu);
        JMenuItem open = new JMenuItem("Open");
        open.addActionListener(this);
        JMenuItem save = new JMenuItem("Save");
        save.addActionListener(this);
        JMenuItem print = new JMenuItem("Print");
        print.addActionListener(this);
        JMenuItem newfile = new JMenuItem("New");
        newfile.addActionListener(this);
        fileMenu.add(open);
        fileMenu.add(save);
        fileMenu.add(print);
        fileMenu.add(newfile);


        JMenuItem cut = new JMenuItem("cut");
        cut.addActionListener(this);
        JMenuItem copy = new JMenuItem("copy");
        copy.addActionListener(this);
        JMenuItem paste = new JMenuItem("paste");
        paste.addActionListener(this);
        JMenuItem undo = new JMenuItem("undo");
        undo.addActionListener(this);
        editMenu.add(cut);
        editMenu.add(copy);
        editMenu.add(paste);
        Frame.setJMenuBar(menuBar);
        Frame.show();

    }

    public static void main(String[] args) {
        simpleTextEditor simpleTextEditor=new simpleTextEditor();
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        String s =e.getActionCommand();
        if(s=="cut") Textarea.cut();
        else if(s=="paste") Textarea.paste();
        else if(s=="copy") Textarea.copy();
        else if (s=="Save") {
            JFileChooser jFileChooser = new JFileChooser("C:");
            int ans=jFileChooser.showOpenDialog(null);
            if(ans==JFileChooser.APPROVE_OPTION){
                File file=new File(jFileChooser.getSelectedFile().getAbsolutePath());
                BufferedWriter writer= null;
                try {
                    writer = new BufferedWriter(new FileWriter(file,false));
                } catch (IOException ex) {
                    throw new RuntimeException(ex);
                }
                try {
                    writer.write(Textarea.getText());
                } catch (IOException ex) {
                    throw new RuntimeException(ex);
                }
                try{
                    writer.flush();
                }catch (IOException ex){
                    throw new RuntimeException(ex);
                }
                try {
                    writer.close();
                } catch (IOException ex) {
                    throw new RuntimeException(ex);
                }
            }
        }else if(s=="print file"){
            try {
                Textarea.print();
            } catch (PrinterException ex){
                throw new RuntimeException(ex);
            }
        }else if(s=="Open"){
            JFileChooser JFileChooser = new JFileChooser("C:");
            int ans=JFileChooser.showOpenDialog(null);
            if(ans==JFileChooser.APPROVE_OPTION){
                File file = new File(JFileChooser.getSelectedFile().getAbsolutePath());
                try {
                    String s1="",s2="";
                    BufferedReader BufferReader = new BufferedReader(new FileReader(file));
                    s2=BufferReader.readLine();
                    while ((s1=BufferReader.readLine())!=null)
                        s2+=s1+"\n";
                    Textarea.setText(s2);
                } catch (IOException ex) {
                    throw new RuntimeException(ex);
                }
            }
        }
    }
}
