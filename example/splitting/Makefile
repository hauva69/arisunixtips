sed:
	sed 's/, / /g' < fibonacci.in

cut:
	sed 's/, / /g' < fibonacci.in | cut -f 4 -d ' '

cut-with-tab:
	sed 's/, / /g' < fibonacci_with_tab.in | cut -f 4 -d ' '

cut-not-working:
	sed 's/, / /g' < fibonacci.in | cut -f 5 -d ', ' 
	
awk: cut cut-with-tab
	sed 's/, / /g' < fibonacci_with_tab.in | awk '{ print $$4 }'
	sed 's/, / /g' < fibonacci.in | awk '{ print $$4 }'
