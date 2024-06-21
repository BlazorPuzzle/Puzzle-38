# Blazor Puzzle #38

## Link the Weather

YouTube Video: https://youtu.be/Sz4RxQ8_czo

Blazor Puzzle Home Page: https://blazorpuzzle.com

### The Challenge:

This is a .NET 8 Global WebAssembly Blazor Web App that lets you download weather data from the National Weather Service. 

The challenge is to add a feature whereby the user can share the results of a zone search resulting in the weather forecast by creating a permalink to it.

### The Solution:

The solution is surprisingly simple. We added the following parameter, z, which we pull from the query string:

```c#
[Parameter]
[SupplyParameterFromQuery(Name = "z")]
public string SelectedLocation { get; set; } = string.Empty;
```

Then we implement `OnParametersSetAsync()` and use it to select a zone. If it's valid, we call `SelectZone` passing it in.

```c#
protected override async Task OnParametersSetAsync()
{
    if (!string.IsNullOrEmpty(SelectedLocation))
    {
        var thisZone = AllZones.FirstOrDefault(z => z.Key.Equals(SelectedLocation, StringComparison.InvariantCultureIgnoreCase));
        if (thisZone is not null) await SelectZone(thisZone);
    }
    await base.OnParametersSetAsync();
}
```

Boom!
