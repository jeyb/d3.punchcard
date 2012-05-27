# d3.punchcard
Inspired by GitHub's punchcard, I created one to track information in my applications.

![d3.punchcard](http://f.cl.ly/items/3c1A3o3S1z1O3c1p132l/punchcard.png)

#### Footnote

Tracking data is important in apps, and while there is great tracking libraries and products, sometimes it's fun to do it yourself. If you wan't to track registrations for your app over the last week, you can do the following via Ruby on Rails and dump the data to the JavaScript file.

```ruby
week = Time.now.beginning_of_week - 1.week
days = [week.strftime('%Y-%m-%d')]
1.upto(6) do |i|
  days << (week + i.days).strftime('%Y-%m-%d')
end

data = days.map do |day|
  by_hour = Array.new(24) { 0 }
  User.
    where('date(created_at) = ?', day).
    select('hour(created_at) as hour, count(*) as count').
    group('hour(created_at)').
    order('hour(created_at)').
    map { |user| by_hour[user.hour.to_i] = user.count.to_i }

  by_hour
end

```