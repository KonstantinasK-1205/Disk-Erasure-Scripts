#!/bin/bash
OutputHorizontalLine(){
	printf '%*s\n' "${COLUMNS:-$(stty size | awk '{printf $2}')}" '' | tr ' ' -
	echo -ne ${_DEFAULT}
}

CheckRenamePackage(){
	local RenameHelp=$(rename -V)
	if [[ "$RenameHelp" == *"/usr/bin/rename"* ]]; then
		RenameType="Perl"
	elif [[ "$RenameHelp" == *"util-linux"* ]]; then
		RenameType="Unix"
	else
		RenameType="Non"
	fi
}

RenameFunc(){
	if [[ -z "$3" ]]; then Change=*.pdf; else Change=$3; fi
	if [[ "$RenameType" = "Unix" ]]; then
		rename "$1" "$2" $Change
	elif [[ "$RenameType" = "Perl" ]]; then
		rename 's/$1/$2/' $Change
	fi
}

FixDamnNames(){
	MaxtorDrives=$(ls -R | grep "Maxtor")
	IFS=$'\n'
	for Maxtor in $MaxtorDrives; do
		OriginalSN=$(ls | grep Maxtor | grep -o "(.*.)" | tr -d '()')
		ChangedSN=$(ls | grep Maxtor | grep -o "(.*.)" | tr -d '()' | grep -o "S-N.* " | tr -d [:space:])
		export OriginalSN
		export ChangedSN
		RenameFunc "Q$ENV{OriginalSN}" "$ENV{ChangedSN}" $Maxtor
	done

	local DiskSizes=$(ls -R | grep -o "\- [0-9].*B" | grep -o "[0-9].*B" | sort | uniq | sed 's/[0-9]GB/ GB/g')
	IFS=$'\n'
	for Size in $DiskSizes; do
		(
			if [[ "$Size" = "14 9 GB" ]]; then
				RenameFunc "14 9 GB" "16 GB"

			elif [[ "$Size" = "28 8 GB" ]]; then
				RenameFunc "28 8 GB" "32 GB"

			elif [[ "$Size" = "29 8 GB" ]]; then
				RenameFunc "29 8 GB" "32 GB"

			elif [[ "$Size" = "46 6 GB" ]]; then
				RenameFunc "46 6 GB" "50 GB"

			elif [[ "$Size" = "59 6 GB" ]]; then
				RenameFunc "59 6 GB" "64 GB"

			elif [[ "$Size" = "74 5 GB" ]]; then
				RenameFunc "74 5 GB" "80 GB"

			elif [[ "$Size" = "93 2 GB" ]]; then
				RenameFunc "93 2 GB" "100 GB"

			elif [[ "$Size" = "112 GB" ]]; then
				RenameFunc "112 GB" "120 GB"

			elif [[ "$Size" = "119 GB" ]]; then
				RenameFunc "119 GB" "120 GB"

			elif [[ "$Size" = "149 GB" ]]; then
				RenameFunc "149 GB" "160 GB"

			elif [[ "$Size" = "168 GB" ]]; then
				RenameFunc "168 GB" "180 GB"

			elif [[ "$Size" = "179 GB" ]]; then
				RenameFunc "179 GB" "180 GB"

			elif [[ "$Size" = "224 GB" ]]; then
				RenameFunc "224 GB" "250 GB"

			elif [[ "$Size" = "233 GB" ]]; then
				RenameFunc "233 GB" "250 GB"

			elif [[ "$Size" = "234 GB" ]]; then
				RenameFunc "234 GB" "250 GB"

			elif [[ "$Size" = "238 GB" ]]; then
				RenameFunc "238 GB" "250 GB"

			elif [[ "$Size" = "279 GB" ]]; then
				RenameFunc "279 GB" "320 GB"

			elif [[ "$Size" = "298 GB" ]]; then
				RenameFunc "298 GB" "320 GB"

			elif [[ "$Size" = "335 GB" ]]; then
				RenameFunc "335 GB" "320 GB"

			elif [[ "$Size" = "447 GB" ]]; then
				RenameFunc "447 GB" "500 GB"

			elif [[ "$Size" = "466 GB" ]]; then
				RenameFunc "466 GB" "500 GB"

			elif [[ "$Size" = "477 GB" ]]; then
				RenameFunc "477 GB" "500 GB"

			elif [[ "$Size" = "596 GB" ]]; then
				RenameFunc "596 GB" "640 GB"

			elif [[ "$Size" = "699 GB" ]]; then
				RenameFunc "699 GB" "750 GB"

			elif [[ "$Size" = "932 GB" ]]; then
				RenameFunc "932 GB" "1000 GB"

			elif [[ "$Size" = "1 36 TB" ]]; then
				RenameFunc "1 36 TB" "1500 GB"

			elif [[ "$Size" = "1 82 TB" ]]; then
				RenameFunc "1 82 TB" "2000 GB"

			elif [[ "$Size" = "2 73 TB" ]]; then
				RenameFunc "2 73 TB" "3000 GB"
			fi
		) &
	done && wait
}

DuplicateChecker(){
	DuplicateList=$(ls -R | grep -o "(.*.) " | sort | uniq -d)
	AmountOfDuplicates=$(echo "$DuplicateList" | wc -l)
	if [[ ! "$AmountOfDuplicates" = 0 && "$DuplicateList" == *"S-N"* ]]; then
		IFS=$'\n'
		for DuplicateEntry in $DuplicateList; do
			(
				Duplicate=`find -name "*$DuplicateEntry*" | head -n 1` &&
				echo "Duplicate Found: "$DuplicateEntry"- Deleting!" &&
				rm -rf "$Duplicate"
			) &
		done && wait && echo "Total of $AmountOfDuplicates duplicate(-s) was deleted"
	else
		echo -e "No Duplicates found! Continuing...                                                                                    "\\r
	fi
}

Mover(){
	mv *"$1"*.pdf "$2"/ 2>/dev/null
}

FactorSorter(){
	OutputHorizontalLine
	mkdir -p "2.5 inch" && mkdir -p "3.5 inch" && mkdir -p "Unknown"
	echo -e "Please wait while we sort files to respective folders! ( Factor Size )" \\r
	LaptopHDD=0
	DesktopHDD=0
	UnknownHDD=0
	TotalAmount=$(ls *.pdf | wc -l)
	for Certificate in *.pdf; do
		for HDDModel in $Models; do
			HDDModel=$(echo $HDDModel | cut -d '@' -f 1)
			if [[ "$Certificate" == *"$HDDModel"* ]]; then
				Factor=$(echo "$Models" | grep "$HDDModel" | cut -d '@' -f 2)
				if [[ "$Factor" == "2.5\"" ]]; then
					mv "$Certificate" "2.5 inch" 2>/dev/null &&
					LaptopHDD=$((LaptopHDD+1))
					tput cup 5 0
					echo -e "Moving to 2.5 folder [ $LaptopHDD / $TotalAmount ]"\\r
					break 1;
				elif [[ "$Factor" == "3.5\"" ]]; then
					mv "$Certificate" "3.5 inch" 2>/dev/null &&
					CurrentNumberCurrentNumber=$((CurrentNumberCurrentNumber+1))
					tput cup 6 0
					echo -e "Moving to 3.5 folder [ $CurrentNumberCurrentNumber / $TotalAmount ]"\\r
					break 1;
				else
					mv "$Certificate" "Unknown" 2>/dev/null &&
					UnknownHDD=$((UnknownHDD+1))
					tput cup 7 0
					echo -e "Moving to Unknown folder [ $UnknownHDD / $TotalAmount ]"\\r
					break 1;
				fi
			fi
		done
	done
	# Leftovers
	for Certificate in *.pdf; do
		mv "$Certificate" "Unknown" 2>/dev/null &&
		UnknownHDD=$((UnknownHDD+1))
		tput cup 7 0
		echo -e "Moving to Unknown folder [ $UnknownHDD / $TotalAmount ]"\\r
	done
	
	tput cup 8 0
	find . -type d -empty -delete
	echo ""
}

SizeSorter(){
	DiskSizes=$(ls -R | grep -o "\- [0-9].*B" | tr -d "\- " | sort | uniq | sed 's/GB/ GB/g' | sed 's/TB/ TB/g')
	echo -e "Managing $(basename "$(pwd)\"")\t\t "\\r	
	for Size in $DiskSizes; do
		mkdir -p "$Size"
		echo -ne "\tManaging $Size | "
		Mover $Size "$Size"

		cd "$Size" > /dev/null
		DiskAmount=$(ls | grep ".pdf" | wc -l)
		HDDStateSorter
		ManufactorSorter
		cd .. > /dev/null
		echo -e "$DiskAmount pcs."
		echo -e "\t\tDone!"\\r	
	done
}

HDDStateSorter(){
	mkdir -p Blogi
	for File in *.pdf; do
		(
			pdftotext "$File" "$File.txt" &&
			if grep -q "FAILED" "$File.txt"; then
				mv "$File" Blogi/
			fi &&
			rm -rf "$File.txt"
		) &
	done && wait
}

ManufactorSorter(){
	mkdir -p WD Toshiba Samsung Seagate HGST Hitachi Fujitsu Maxtor Intel
	shopt -s nocaseglob
	Mover WDC WD &
	Mover Hitachi Hitachi &
	Mover SAMSUNG Samsung &
	Mover FUJITSU Fujitsu &
	Mover TOSHIBA Toshiba &
	Mover HGST HGST &
	Mover ST Seagate &
	Mover Maxtor Maxtor &
	Mover ADATA ADATA &
	Mover Corsair Corsair &
	Mover SanDisk SanDisk &
	Mover Intel Intel &
	Mover LITEONIT LiteOnIt &
	Mover Micron Micron &
	tput sc
	echo -ne "\tMoving $File      "\\r
	tput rc; tput el
	wait
}
clear && echo -en "\e[3J"

# Find all certificates within folder with extension .pdf,
# Not a single certificate was found, search for .tar archive
PDFs=$(find . -name '*.pdf')
if [[ -z "$PDFs" && $(find . -name '*.tar') ]]; then
	for tar in *.tar; do
		tar xf $tar
	done && rm -rf $tar
	PDFs=$(find . -name '*.pdf')
fi

# Sources from which to get information about HDD Certificates ( Form Factor )
HDD_DB=$(cat ~/Documents/Scripts/HDDDataBase.txt)
Models=$(echo -e "$HDD_DB")
if [ -f *".txt" ]; then
	Log=$(cat $(find . -name "*.txt") | awk -F "@" '{print $2 "@" $7}')
	Models=$(echo -e "$Log" "$HDD_DB")
fi

CheckRenamePackage
DuplicateChecker
FixDamnNames

DateRanges=$(ls -R | grep -o "201[7-9]-..-..")
DiskSizes=$(ls -R | grep -o "\- [0-9].*B" | tr -d "\- " | sort | uniq | sed 's/GB/ GB/g' | sed 's/TB/ TB/g')
IFS=$'\n'
if [[ "{$1,,}" == "-auto" || -z "$1" ]]; then
	FactorSorter	
	for FactorSize in */; do
		cd "$FactorSize" > /dev/null
		SizeSorter
		cd .. > /dev/null
	done
	cd .. > /dev/null
	
elif [[ "$1" == "-x" ]]; then
	for pdf in $PDFs; do
		mv "$pdf" . 2> /dev/null
	done && echo "Done!"
fi

echo -ne ""

find . -type d -empty -delete
