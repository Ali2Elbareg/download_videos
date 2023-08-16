from pytube import YouTube
from tkinter import*
from tkinter import filedialog
import threading
from PIL import Image, ImageTk
from urllib.request import urlopen
import io

url = r"https://as1.ftcdn.net/v2/jpg/01/63/59/46/1000_F_163594682_X1SfMzyVpj8h71I5LEzfh1VjLzbul6U8.jpg"
image_url = r"https://lh4.googleusercontent.com/kkYl5eJg7PEYSeUb1EaHEgI1ce1DyY-hu_X0nYOLvdc5iG721bF0_GJIXJBNVI8Jjxo-76E6fBzeWF6HLQyNPqUTjKUigdodYmP6JQXYlQ8BBZyIsMHctMhPokkmgwuKv0cz5bWT"
# image_url = r"https://lh4.googleusercontent.com/Sa75N_Uq-pLg6Ap6SAugqZX_fLg9XGob7tpUa5w5CHZM8KV-71AERxbxVDsw61fjR44J2Q25090j2WYcazeq13g3BXQK4X_GikzyH4eraW8UI18RAIRcOjeHxKucu3r_o666Suea"

root = Tk()

response1 = urlopen(image_url)
image_data = response1.read()
image = Image.open(io.BytesIO(image_data))


desired_width = 200
desired_height = 150
resized_image = image.resize((desired_width, desired_height), Image.ANTIALIAS)

tk_image = ImageTk.PhotoImage(resized_image)


response2 = urlopen(url)
logo_data = response2.read()
logo_image = Image.open(io.BytesIO(logo_data))
tk_logo = ImageTk.PhotoImage(logo_image)
root.iconphoto(True, tk_logo)

root.title('Youtube Downloader')
root.geometry('600x320')
root.resizable(False, FALSE)

# My Function


def browse():
    directory = filedialog.askdirectory(title='Save Video')
    folderLink.delete(0, 'end')
    folderLink.insert(0,directory)


def down_yt():
    status.config(text='Status: Downloading ...')
    link = ytlink.get()
    folder = folderLink.get()
    YouTube(link, on_complete_callback=finish).streams.filter(progressive=True, file_extension='mp4').order_by('resolution').desc().first().download(folder)


def finish(stream=None, chunck=None, file_handle=None, remaining=None):
    status.config(text='Status: Complete')


# Youtube Logo

# ytLogo = PhotoImage(file='youtube logo.png').subsample(2)
ytTitle = Label(root, image=tk_image)
ytTitle.place(relx=0.5, rely=0.25, anchor='center')


#Youtube Link

ytLabel = Label(root, text='Youtube Link')
ytLabel.place(x=25, y=150)

ytlink = Entry(root, width=60)
ytlink.place(x=140, y=150)

# Download Folder

folderLabel = Label(root, text='Download Folder')
folderLabel.place(x=25, y=183)

folderLink = Entry(root, width=50)
folderLink.place(x=140, y=183)


# Browse Button

browse = Button(root, text='Browse', command=browse)
browse.place(x=455, y=180)

# Download Button n

download = Button(root, text='Download', command=threading.Thread(target=down_yt).start)
download.place(x=280, y=220)


# Status Bar

status = Label(root, text='Status: Ready', font='Calibre 10 italic', fg='black', bg='white', anchor='w')
status.place(rely=1, anchor='sw', relwidth=1)


root.mainloop()
