---
layout: post
title: 'Programmatic Data Access in Geodesy'
date: 2024-05-22
permalink: /blog/automate-data-access/
---



> Never spend 6 minutes doing something by hand with, when you can spend 6 hours automating it.
 

This will be a tutorial to download common geodetic datasets mainly - Rinex files from their respective ftp servers/APIs.

## RINEX Files

It's always efficient to download the data in an automated and reporducible manner by leveraging the APIs and ftp automation. One can always log into the filezilla and manually search through the directories for the required file, but this becomes teadious when one has to download files for tens and hundreds of days. Here comes programming to the rescue!

I'll be using Python as it's highly flexible, cross platform and boasts a vibrant community of developers both for Geodesy and Geospatial applications.

There are mainly 2 ftp servers hosted by -

1. SCRIPPS Orbit and Parmanent Array Center ([SOPAC](http://sopac-csrc.ucsd.edu/index.php/sopac/)) from SCRIPPS Institute of Oceanorgaphy at University of Californai, San Diego (UCSD).
2. Crustal Dynamics and Data Information System ([CDDIS](https://cddis.nasa.gov/)) from NASA.
---

### Step - 1: Login

A simplified implementation to create a ftp login session.

```python
def login_sopac(username='anonymous', password=''):
    """ SOPAC ftp login 'garner.ucsd.edu' 

    Args:
        username (str): Required to login with 'anonymous'.
        password (str): Enter Email. Defaults to ''.
    """
    try:
        ftps = FTP()
        ftps.connect('garner.ucsd.edu')
        ftps.login(user=username, passwd=password)
        print(ftps.welcome)
        return ftps

    except Exception as e:
        print("Error:", e)  # Catch-all for other issues
```
---

### Step - 2: SOPAC directory structure

The daily rinex files are precent in the `/pub/rinex` directory. File for a particular date and year can be located in the respective `/pub/rinex/{year}/{doy}` directory. After this one has to match the station name using the station id and required rinex version.

```python
FTP_ROOT = '/pub/rinex'
FILE_FTP_DIR = f'{FTP_ROOT}/{year}/{doy}'
```

### Step -3: Download a specific file from ftp

```python
def download(session, fname, ftp_dir, download_dir):
    """Download the required file from any ftp server

    Args:
        session (ftplib.FTP): The login session 
        fname (str): name of the file (same as in ftp server)        ftp_dir (str): path to the directory at ftp from which file needs to be downloaded
        download_dir (str): directory path where the file will be downloaded (locally)
    """
    try:
        print(f"FTP Dir: {session.pwd()}, Local Dir: {os.getcwd()}")

        session.retrbinary("RETR " + fname, open(os.path.join(download_dir, fname), 'wb').write)

        return(f'{os.path.join(download_dir, fname)}')
    except Exception as e:
        print(f"Error occured, {e}")
```

### Step 4: One stop solution

```python

def sopac_rinex(download_dir, session, station, year, doy):
    """Downloads the rinex file for a given station, year and day of year.

    Args:
        download_dir (str): path to download dir (local)
        session (ftp.FTP): login session 
        station (str): official 4 character station code
        year (int): year 
        doy (int): day of year

    Raises:
        ValueError: data not found
    """
    FTP_ROOT = '/pub/rinex'
    doy = str(doy).zfill(3)
    FILE_FTP_DIR = f'{FTP_ROOT}/{year}/{doy}'
    
    session.cwd(FILE_FTP_DIR)
    all_stations = session.nlst()
    datafiles = find_station(all_stations, stn_code=station)

    if len(datafiles) > 0:
        try:
            download(session=session, fname=datafiles[0], ftp_dir=FILE_FTP_DIR, download_dir=download_dir)
        except Exception as e:
            print(e)

    elif len(datafiles) == 0:
        raise ValueError(f"Station code - {station} data for {year} and day {doy} Not Found")
    
```

This can further be used to create a bulk download utility.

```python
def sopac_bulk_rinex(download_dir, station, start_year: int, start_doy: int, end_year: int, end_doy: int):
    """Bulk download rinex files for a given start and end date from SOPAC ftp server.

    Args:
        download_dir (str): path to download dir (local)
        station (str): official 4 character station code
    
    Raises:
        ValueError: data not found
    """

    sopac = login_sopac()

    if start_year == end_year:
        doys = np.arange(start_doy, end_doy+1, 1)
        for _doy in tqdm(doys):
            try:
                sopac_rinex(download_dir=download_dir, session=sopac, station=station, year=start_year, doy=_doy)
            except ValueError as e:
                print(f"File Not Found - {station} data for {start_year} and day {_doy} - skipping!")

    elif start_year > end_year:
        raise ValueError("Invalid Input: end year should be higher than starting year")
    
    else:
        years = np.arange(start_year, end_year+1, 1)
        for yr in tqdm(years):
            if yr == start_year:
                doys = np.arange(start_doy, 366+1, 1)
                for _doy in tqdm(doys):
                    try:
                        sopac_rinex(download_dir=download_dir, session=sopac, station=station, year=start_year, doy=_doy)
                    except ValueError as e:
                        print(f"File Not Found - {station} data for {start_year} and day {_doy} - skipping!")

            if yr > start_year and yr < end_year:
                doys = np.arange(1, 366+1, 1)
                for _doy in tqdm(doys):
                    try:
                        sopac_rinex(download_dir=download_dir, session=sopac, station=station, year=start_year, doy=_doy)
                    except ValueError as e:
                        print(f"File Not Found - {station} data for {start_year} and day {_doy} - skipping!")

            if yr == end_year:
                doys = np.arange(1, end_doy+1, 1)
                for _doy in tqdm(doys):
                    try:
                        sopac_rinex(download_dir=download_dir, session=sopac, station=station, year=start_year, doy=_doy)
                    except ValueError as e:
                        print(f"File Not Found - {station} data for {start_year} and day {_doy} - skipping!")

```

These are a simplified first order solution, further iterations will be required to improve the code. Nonetheless it is a good starting point to get things done!!
