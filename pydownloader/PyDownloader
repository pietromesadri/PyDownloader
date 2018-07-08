#!/usr/bin/python3
# -*- coding: utf-8 -*-


import sys, youtube_dl

from PyQt5 import QtCore

from PyQt5.QtWidgets import (QMainWindow, QLineEdit,
                             QAction, QApplication,
                             QWidget, QPushButton,
                             QGridLayout, QLabel)

from PyQt5.QtGui import QIcon, QFont

class DownloadThread(QtCore.QThread):

    data_downloaded = QtCore.pyqtSignal(object)

    def __init__(self, url, options):
        QtCore.QThread.__init__(self)
        self.url = url
        self.options = options

    def run(self):
        with youtube_dl.YoutubeDL(self.options) as downloader:
            downloader.download([self.url])



class PyDownloader(QWidget):
    def __init__(self):
        super().__init__()

        self.initUI()

    def initUI(self):

        grid = QGridLayout()
        grid.setSpacing(10)

        urlLabel = QLabel('URL')
        self.statusLabel = QLabel('Ready to work!')
        self.URL = QLineEdit(self)
        downButton = QPushButton('Download')

        grid.addWidget(urlLabel, 1, 0)
        grid.addWidget(self.URL, 2, 0, 1, 50)
        grid.addWidget(downButton, 3, 0)
        grid.addWidget(self.statusLabel, 4, 0)


        downButton.clicked.connect(self.download)

        self.setLayout(grid)

        self.move(300, 300)
        self.setWindowTitle('PyDownloader')
        self.show()

    def hook(self, d):
        if d['status'] == 'downloading':
            self.statusLabel.setText("Downloading...")

        elif d['status'] == 'finished':
            self.statusLabel.setText("Finished downloading!")

        elif d['status'] == 'error':
            self.statusLabel.setText("Something went wrong!")
        else:
            self.statusLabel.setText("Ready to work!")

    def download(self):
        # youtubedl code
        url = self.URL.text()
        options = {
            'format': 'best',
            'progress_hooks': [self.hook],
        }
        self.threads = []
        downloader = DownloadThread(url, options)
        self.threads.append(downloader)
        downloader.start()

if __name__ == '__main__':

    app = QApplication(sys.argv)
    PyDownloader = PyDownloader()
    sys.exit(app.exec_())
