from tkinter import *
import tkinter as tk
from PIL import ImageTk, Image

root = Tk()
root.iconphoto(False, tk.PhotoImage(file='covid.png'))

root.geometry("800x600")
root.minsize(800,600)
root.title("Covid-19 Country Status")
root['background'] = 'ORANGE'
canvas = Canvas(width=596, height=200)
canvas.pack()
img = ImageTk.PhotoImage(Image.open("z.jpg"))
canvas.create_image(1, 1, anchor=NW, image=img)

def get_country_data():
    import requests
    import json
    url = 'https://api.covid19api.com/summary'
    response = requests.request("GET",url)

    data = json.loads(response.text)
    
    searchcountry = txt.get()

    def get_country_index(country):
        for index,item in enumerate(data['Countries']):
            if item['Country'] == country:
                return index

    countryid = get_country_index(searchcountry)

    totalconfirmed = data['Countries'][countryid]['TotalConfirmed']
    totalrecovered = data['Countries'][countryid]['TotalRecovered']
    newconfirmed = data['Countries'][countryid]['NewConfirmed']
    totaldeath = data['Countries'][countryid]["TotalDeaths"]

    covid_msg = f'\nThe total cases are: {totalconfirmed}.\nThe total recovered are: {totalrecovered}.\nNew confirmed cases are: {newconfirmed}.\nThe total deaths are: {totaldeath}'

    output_text.set(covid_msg)
   
def showdata():
    from matplotlib import pyplot as plt
    import matplotlib.patches as mpatches
    from covid import Covid
    covid = Covid()
    cases = []
    confirmed = []
    active = []
    deaths = []
    recovered = []

    try:
        root.update()
        countries = txt.get()
        country_names = countries.strip()
        country_names = country_names.replace(" ", ",")
        country_names = country_names.split(",")

        for x in country_names:
            cases.append(covid.get_status_by_country_name(x))
            root.update()
        for y in cases:
            confirmed.append(y["confirmed"])
            active.append(y["active"])
            deaths.append(y["deaths"])
            recovered.append(y["recovered"])

        confirmed_patch = mpatches.Patch(color='black', label='Confirmed')
        recovered_patch = mpatches.Patch(color='green', label='Recovered')
        active_patch = mpatches.Patch(color='red', label='Active')
        deaths_patch = mpatches.Patch(color='yellow', label='Deaths')
        plt.legend(handles=[confirmed_patch, recovered_patch, active_patch, deaths_patch])

        for x in range(len(country_names)):
            plt.bar(country_names[x], confirmed[x], color='black')
            if recovered[x] > active[x]:
                plt.bar(country_names[x], recovered[x], color='green')
                plt.bar(country_names[x], active[x], color='red')
            else:
                plt.bar(country_names[x], active[x], color='red')
                plt.bar(country_names[x], recovered, color='green')
        plt.bar(country_names[x], deaths[x], color='yellow')
        plt.title('Current Covid Cases')
        plt.xlabel('Country Name')
        plt.ylabel('cases(in Crores)')
        plt.show()
    except Exception as e:
        print("Enter Correct Details")

Label(root, text="Covid-19 data", font="Timesnewroman 17 bold",
      bg='ORANGE').pack()
Label(root, text="\n Enter Country Name", font="Timesnewroman 12 bold", bg='ORANGE').pack()
data = StringVar()
data.set("")
Label(root, bg='ORANGE').pack()

txt = Entry(root,font = "calibre 18 normal" ,width=30)
txt.pack()

output_text = StringVar()
lbl_output = Label(root, textvariable=output_text, font="Consolas 16 bold", height = 5, bg='Orange')
lbl_output.pack()

nbtn = tk.Button(root, text = "Get Data", font="Consolas 15 bold", height=2, width=10, relief = 'solid', command=get_country_data)
nbtn.pack(side = "left",padx=180,fill = X)

gbtn = tk.Button(root, text="Show graph", font="Consolas 15 bold", height=2, width=10, relief = 'solid', command=showdata)
gbtn.pack(side = "left",fill = X)

root.mainloop()
