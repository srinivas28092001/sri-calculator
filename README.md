
import sys
from PyQt5.QtWidgets import (
    QApplication, QWidget, QVBoxLayout,
    QLineEdit, QGridLayout, QPushButton
)
from PyQt5.QtGui import QFont
from PyQt5.QtCore import Qt


class Calculator(QWidget):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Sri Calculator")
        self.setFixedSize(300, 400)
        self.setStyleSheet("background-color: #2c3e50; color: white;")

        self.create_ui()

    def create_ui(self):
        # Main layout
        main_layout = QVBoxLayout()
        self.setLayout(main_layout)

        # Display
        self.display = QLineEdit()
        self.display.setFont(QFont("Arial", 24))
        self.display.setAlignment(Qt.AlignRight)
        self.display.setStyleSheet("background-color: #34495e; padding: 10px; border: none;")
        self.display.setReadOnly(True)
        main_layout.addWidget(self.display)

        # Button layout
        buttons_layout = QGridLayout()

        # Buttons
        buttons = [
            ('7', '8', '9', '/'),
            ('4', '5', '6', '*'),
            ('1', '2', '3', '-'),
            ('0', '.', 'C', '+'),
            ('=',)
        ]

        # Create buttons
        for row, keys in enumerate(buttons):
            for col, key in enumerate(keys):
                button = QPushButton(key)
                button.setFont(QFont("Arial", 18))
                button.setStyleSheet("""
                    QPushButton {
                        background-color: #1abc9c;
                        border: none;
                        padding: 20px;
                        border-radius: 10px;
                    }
                    QPushButton:pressed {
                        background-color: #16a085;
                    }
                """)
                button.clicked.connect(self.on_button_click)
                buttons_layout.addWidget(button, row, col)

        main_layout.addLayout(buttons_layout)

    def on_button_click(self):
        button = self.sender()
        key = button.text()

        if key == "C":
            self.display.clear()
        elif key == "=":
            try:
                result = eval(self.display.text())
                self.display.setText(str(result))
            except:
                self.display.setText("Error")
        else:
            self.display.setText(self.display.text() + key)


if __name__ == "__main__":
    app = QApplication(sys.argv)
    calculator = Calculator()
    calculator.show()
    sys.exit(app.exec_())



