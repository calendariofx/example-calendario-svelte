<div class="container">
    <div class="custom-calendar-wrap custom-calendar-full">
        <div class="custom-header clearfix">
            <h2>Calendario 
                <span><span>Example Svelte</span></span>
            </h2>
            <h3 class="custom-month-year">
                <span id="custom-month" class="custom-month"></span>
                <span id="custom-year" class="custom-year"></span>
                <nav>
                    <span id="custom-prev" class="custom-prev" on:click={() => cal = cal.previousMonth() } title="Go to previous month"></span>
                    <span id="custom-next" class="custom-next" on:click={() => cal = cal.nextMonth() } title="Go to next month"></span>
                    <span id="custom-current" class="custom-current" on:click={() => cal = cal.now() } title="Go to current date"></span>
                </nav>
            </h3>
        </div>
        <div id="calendar" class="fc-calendar-container">
            <div class="fc-calendar" 
                class:fc-four-rows={rows === 4} 
                class:fc-five-rows={rows === 5} 
                class:fc-six-rows={rows === 6}>

                <div class="fc-head">
                    {#each cal.head as weekday}
                        <div>{weekday}</div>
                    {/each}
                </div>

                <div class="fc-body">
                    {#each Array.from(Array(rows).keys()) as ri}
                        <div class="fc-row">
                            {#each body[ri] as dt}
                                <div 
                                    class="{dt.dateClass} {dt.weekday}" 
                                    class:fc-past={dt.past}
                                    class:fc-future={dt.future}
                                    class:fc-today={dt.today}
                                    class:fc-content={dt.events.length > 0}>
                                    <span class="fc-date" class:fc-emptydate={dt.events.length === 0}>{dt.date.day}</span>
                                    <span class="fc-weekday">{dt.weekday}</span>
                                    
                                    <div class="fc-calendar-events">
                                        {#each dt.events as event}
                                            <div class="fc-calendar-event">
                                                {#if event.url}
                                                    <a class="{event.category}" href="{event.url}">{event.content}</a>
                                                {:else}
                                                    <span class="{event.category}">{event.content}</span>
                                                {/if}
                                            </div>
                                        {/each}
                                    </div>
                                </div>
                            {/each}
                        </div>
                    {/each}
                </div>
            
            </div>
        </div>
    </div>
</div>

<script context="module">
    import calendario from 'calendariofx-calendario'
</script>

<script>
    let cal = calendario({
        '01-01-1970' : [{content: 'New Year\'s', repeat: 'YEARLY', allDay: true, endDate: '12-31-2100'}],
        '12-25-1970' : [{content: 'Christmas Day', repeat: 'YEARLY', allDay: true, endDate: '12-31-2100'}], 
        '11-04-2013' : [{content: 'Islamic New Year', repeat: 'YEARLY', allDay: true, endDate: '12-31-2100'}],
        '08-21-2019' : [{content: 'Great American Smokeout', url: 'http://www.wincalendar.com/Great-American-Smokeout', allDay: true}],
        '08-13-2019' : [{content: 'Ashura [An example of an complete date event (11-13-2013)]', allDay: true}],
        '11-11-2019' : [{content: 'Remembrance Day (Canada)', url: 'http://www.wincalendar.com/Remembrance-Day', allDay: true}],
        '08-02-2019' : [
            {content: 'Yeah Monthly', repeat: 'MONTHLY', allDay: true, endDate: '09-02-2019'},
            {content : 'Graduation Exams', repeat: 'INTERVAL', allDay: true, endDate: '08-20-2019'}
        ],
        '01-07-2019' : [{content: 'Monthly And Yearly', repeat: 'MONTHLY', allDay: true, endDate: '02-07-2020'}],
        '08-01-2019' : [{content : 'MONDAY (WEEKLY)', repeat: 'MON', allDay: true, endDate: '08-30-2019'}]
    })

    $: rows = cal.rows
    $: body = cal.body
</script>