import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QToolBar, QAction, QLineEdit
from PyQt5.QtWebEngineWidgets import QWebEngineView
from PyQt5.QtCore import QUrl

class MainWindow(QMainWindow):
    def __init__(self): 
        super(MainWindow, self).__init__()
        self.browser = QWebEngineView()
        self.browser.setUrl(QUrl("http://www.google.com"))
        self.setCentralWidget(self.browser)
        self.showMaximized()

        # Navbar
        self.navbar = QToolBar()
        self.addToolBar(self.navbar)

        # Back button
        back_button = QAction('Back', self)
        back_button.triggered.connect(self.browser.back)
        self.navbar.addAction(back_button)

        # Forward button
        forward_button = QAction('Forward', self)
        forward_button.triggered.connect(self.browser.forward)
        self.navbar.addAction(forward_button)

        # Reload button
        reload_button = QAction('Reload', self)
        reload_button.triggered.connect(self.browser.reload)
        self.navbar.addAction(reload_button)

        # home button
        home_button = QAction('Home', self)
        home_button.triggered.connect(self.browser.home)
        self.navbar.addAction(home_button)

        # URL bar
        self.url_bar = QLineEdit()
        self.url_bar.returnPressed.connect(self.navigate_to_url)
        self.navbar.addWidget(self.url_bar)

        # Update URL bar when page changes
        self.browser.urlChanged.connect(self.update_url)

    def navigate_to_url(self):
        url = self.url_bar.text()
        if not url.startswith("http"):
            url = "http://" + url
        self.browser.setUrl(QUrl(url))

    def update_url(self, q):
        self.url_bar.setText(q.toString())

# Main execution
app = QApplication(sys.argv) 
QApplication.setApplicationName('Warda Mukhtar Browser')
window = MainWindow()
window.show()
app.exec()
