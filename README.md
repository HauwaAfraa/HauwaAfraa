import javax.swing.*;
import javax.swing.filechooser.FileNameExtensionFilter;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.io.File;
import java.io.FileNotFoundException;
import java.io.PrintWriter;
import java.util.Scanner;

public class WordCounter extends JFrame {

    private JTextArea textArea;
    private JLabel wordCountLabel, paragraphCountLabel, lineCountLabel, titleLabel;

    public WordCounter() {
        super("Word Counter By the Alphas");
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(400, 300);
        setLayout(new BorderLayout());
 
        titleLabel = new JLabel("The Alphas for CPS301", SwingConstants.CENTER);
        add(titleLabel, BorderLayout.NORTH);

        textArea = new JTextArea();
        JScrollPane scrollPane = new JScrollPane(textArea);
        add(scrollPane, BorderLayout.CENTER);

        JPanel buttonPanel = new JPanel(new FlowLayout());
        JButton openButton = new JButton("Open");
        JButton countButton = new JButton("Count");
        JButton saveButton = new JButton("Save");

        wordCountLabel = new JLabel("Words: 0");
        paragraphCountLabel = new JLabel("Paragraphs: 0");
        lineCountLabel = new JLabel("Lines: 0");

        buttonPanel.add(openButton);
        buttonPanel.add(countButton);
        buttonPanel.add(saveButton);
        buttonPanel.add(wordCountLabel);
        buttonPanel.add(paragraphCountLabel);
        buttonPanel.add(lineCountLabel);

        add(buttonPanel, BorderLayout.SOUTH);

        openButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                JFileChooser fileChooser = new JFileChooser();
                fileChooser.setFileFilter(new FileNameExtensionFilter("Text files", "txt"));
                int result = fileChooser.showOpenDialog(WordCounter.this);
                if (result == JFileChooser.APPROVE_OPTION) {
                    File selectedFile = fileChooser.getSelectedFile();
                    try {
                        Scanner scanner = new Scanner(selectedFile);
                        StringBuilder stringBuilder = new StringBuilder();
                        while (scanner.hasNextLine()) {
                            stringBuilder.append(scanner.nextLine()).append("\n");
                        }
                        textArea.setText(stringBuilder.toString());
                    } catch (FileNotFoundException ex) {
                        ex.printStackTrace();
                    }
                }
            }
        });

        countButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                String text = textArea.getText();
                int wordCount = text.split("\\s+").length;
                int paragraphCount = text.split("\n\n").length;
                int lineCount = text.split("\n").length;
                wordCountLabel.setText("Words: " + wordCount);
                paragraphCountLabel.setText("Paragraphs: " + paragraphCount);
                lineCountLabel.setText("Lines: " + lineCount);
            }
        });

        saveButton.addActionListener(new ActionListener() {
            @Override
            public void actionPerformed(ActionEvent e) {
                JFileChooser fileChooser = new JFileChooser();
                int result = fileChooser.showSaveDialog(WordCounter.this);
                if (result == JFileChooser.APPROVE_OPTION) {
                    File file = fileChooser.getSelectedFile();
                    try (PrintWriter writer = new PrintWriter(file)) {
                        writer.print(textArea.getText());
                    } catch (FileNotFoundException ex) {
                        ex.printStackTrace();
                    }
                }
            }
        });
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                new WordCounter().setVisible(true);
            }
        });
    }
}

<!---
HauwaAfraa/HauwaAfraa is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profi
You can click the Preview link to take a look at your changes.
--->
