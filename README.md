git init -b main

$d = Get-Date "2026-08-01"
$end = Get-Date "2026-10-30"

while ($d -le $end) {
    $day = $d.ToString("yyyy-MM-dd")
    $count = Get-Random -Minimum 1 -Maximum 5   # 1 to 4 commits

    for ($i = 1; $i -le $count; $i++) {
        $hour = Get-Random -Minimum 9 -Maximum 21
        $min = Get-Random -Minimum 0 -Maximum 60
        $stamp = "{0}T{1:D2}:{2:D2}:00" -f $day, $hour, $min

        Add-Content activity.txt "Update $day #$i"
        git add .

        $env:GIT_AUTHOR_DATE = $stamp
        $env:GIT_COMMITTER_DATE = $stamp
        git commit -m "Update $day #$i" --quiet
    }

    $d = $d.AddDays(1)
}

Remove-Item Env:GIT_AUTHOR_DATE, Env:GIT_COMMITTER_DATE
git log --oneline | Measure-Object -Line