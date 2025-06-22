#Find Time Slots with Most 'No Cars Available' Status
SELECT 
  EXTRACT(HOUR FROM timestamp) AS hour_of_day,
  COUNT(*) AS no_cars_count
FROM uber_requests
WHERE status = 'No Cars Avail'
GROUP BY hour_of_day
ORDER BY no_cars_count DESC;

# Cancellations by Drivers in Early Morning
SELECT 
  EXTRACT(HOUR FROM timestamp) AS hour_of_day,
  COUNT(*) AS cancellations
FROM uber_requests
WHERE status = 'Cancelled'
  AND EXTRACT(HOUR FROM timestamp) BETWEEN 4 AND 9
GROUP BY hour_of_day
ORDER BY cancellations DESC;

#Supply Gap at Night (Airport to City)
SELECT 
  pickup_point,
  EXTRACT(HOUR FROM timestamp) AS hour_of_day,
  COUNT(*) AS requests
FROM uber_requests
WHERE pickup_point = 'Airport' 
  AND EXTRACT(HOUR FROM timestamp) BETWEEN 21 AND 4
GROUP BY pickup_point, hour_of_day
ORDER BY requests DESC;
